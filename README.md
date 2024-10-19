
# Depth Map and Point Cloud Estimation

Questo progetto implementa un modello basato su rete neurale U-Net per stimare mappe di profondità da immagini di input. Le mappe di profondità stimate possono essere utilizzate per generare point cloud 3D.

## Requisiti

Prima di eseguire il codice, assicurati di avere:

1. **Python 3.x** installato.
2. Una GPU compatibile con CUDA per accelerare l'addestramento del modello.
3. **Jupyter Notebook** o **Jupyter Lab** per eseguire il notebook `.ipynb`.

### Dipendenze

Installa le dipendenze richieste eseguendo:

```bash
pip install -r requirements.txt
```

Se il file `requirements.txt` non è presente, puoi installare manualmente le librerie essenziali:

```bash
pip install torch torchvision numpy matplotlib tqdm pillow open3d
```

## Struttura del Progetto

Assicurati di avere le seguenti cartelle e file organizzati correttamente:

```
├── depthMap_pointCloud_estimation.ipynb  # Notebook principale
├── immagini/                            # Cartella con le immagini di input
├── mappe_profondità/                    # Cartella con le mappe di profondità ground truth
└── requirements.txt                     # File delle dipendenze (opzionale)
```

- **immagini/**: cartella contenente le immagini di input.
- **mappe_profondità/**: cartella contenente le mappe di profondità ground truth corrispondenti alle immagini.

## Esecuzione del Codice

### 1. Preparazione del Dataset

Organizza il dataset nelle due cartelle:

- `immagini/`: conterrà le immagini RGB (o in scala di grigi) di input.
- `mappe_profondità/`: conterrà le mappe di profondità corrispondenti, che serviranno come ground truth durante l'addestramento.

### 2. Apertura del Notebook

Per avviare il processo:

1. Apri il terminale e lancia Jupyter Notebook o Jupyter Lab:

```bash
jupyter notebook
```

2. Apri il file `depthMap_pointCloud_estimation.ipynb` dal browser.

### 3. Esecuzione del Notebook

Una volta aperto il notebook, segui i seguenti passi per eseguire il codice:

#### a. **Importazioni e Inizializzazioni**

Esegui la prima cella per importare tutte le librerie necessarie, tra cui `torch`, `numpy`, `matplotlib`, e `open3d`.

#### b. **Definizione del Modello**

Il notebook definisce il modello U-Net, utilizzato per la stima della mappa di profondità. Il modello è suddiviso in moduli di convoluzione e upsampling. Le celle seguenti creano l'architettura e ne stampano un riepilogo.

#### c. **Caricamento dei Dati**

Esegui la cella dedicata al caricamento del dataset. Verranno letti i dati dalle cartelle `immagini/` e `mappe_profondità/`. Assicurati che le immagini siano in un formato supportato come `.png` o `.jpg`.

#### d. **Addestramento del Modello**

Esegui la cella di addestramento, che avvierà il training della rete neurale U-Net. Vengono utilizzati la discesa del gradiente (stochastic gradient descent) e la riduzione del tasso di apprendimento tramite lo scheduler `ReduceLROnPlateau`.

L'addestramento produce metriche come la **MSE (Mean Squared Error)** e la **RMSE (Root Mean Squared Error)**, che verranno visualizzate in output.

Assicurati di monitorare il processo di addestramento e i risultati, visualizzando la perdita (loss) a ogni epoca.

#### e. **Valutazione**

Dopo l'addestramento, esegui le celle per la valutazione del modello. Verranno calcolate metriche come:

- **MSE**: Errore quadratico medio.
- **RMSE**: Radice dell'errore quadratico medio.
- **PSNR**: Rapporto segnale-rumore di picco (Peak Signal-to-Noise Ratio).

Il notebook esegue automaticamente la normalizzazione delle mappe di profondità stimate rispetto a quelle ground truth per facilitare il confronto.

#### f. **Generazione della Point Cloud**

Esegui la cella che genera una point cloud a partire dalla mappa di profondità stimata. Utilizza la libreria **Open3D** per visualizzare la point cloud e salvare i risultati in formato `.ply` o `.pcd`.

### 4. Salvataggio e Visualizzazione dei Risultati

- Le **mappe di profondità stimate** possono essere salvate come immagini.
- Le **point cloud** possono essere salvate in formato `.ply` o `.pcd` e visualizzate utilizzando Open3D o un visualizzatore 3D esterno.

### 5. Fine dell'Esecuzione

Al termine dell'addestramento e della valutazione, puoi salvare il modello addestrato per futuri utilizzi o testarlo su nuovi dati di input. 

---

## Esempio di Comando per la Point Cloud

Per generare una point cloud da una mappa di profondità, puoi usare il seguente frammento di codice nel notebook:

```python
depth_image = np.array(Image.open('path_to_depth_image.png'))
point_cloud = o3d.geometry.PointCloud.create_from_depth_image(
    o3d.geometry.Image(depth_image), 
    o3d.camera.PinholeCameraIntrinsic()
)
o3d.visualization.draw_geometries([point_cloud])
```

Questo genererà una point cloud a partire dalla mappa di profondità e la visualizzerà utilizzando Open3D.

---

### Note

- **GPU**: È altamente consigliato eseguire il codice su una GPU per migliorare le prestazioni, specialmente durante l'addestramento.
- **Visualizzazione 3D**: La visualizzazione della point cloud richiede l'installazione della libreria Open3D e un sistema in grado di gestire grafica 3D.
