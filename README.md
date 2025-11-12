# Segmentazione del seno in immagini MRI

Questo repository contiene la tesi di laurea magistrale di **Achille Cannavale** presso l’Università degli Studi di Cassino e del Lazio Meridionale (A.A. 2024/2025).  
Titolo: *Segmentazione del seno in immagini MRI*  
Relatore: Prof. Alessandro Bria  
Correlatore: Ing. Marco Cantone  

---

## 📖 Abstract
La tesi affronta il problema della **segmentazione automatica del seno in immagini di risonanza magnetica (MRI 3D)**, con particolare attenzione al tessuto mammario, fibroghiandolare (FGT) e ai vasi sanguigni.  
Attraverso l’uso di **reti neurali convoluzionali (U-Net, Attention U-Net, SliceUNet)** e del framework **MONAI (Medical Open Network for AI)**, è stata sviluppata una pipeline sperimentale completa, comprensiva di preprocessing, configurazione gerarchica tramite file YAML, logging strutturato e strategie di validazione innovative basate su trasformazioni inverse.  

Il modello finale ha raggiunto un **Dice score di 0.9161**, dimostrando una notevole accuratezza e rilevanza clinica.

---

## 🗂 Struttura della Tesi
- **Introduzione**: contesto, obiettivi e strumenti utilizzati  
- **Background**: evoluzione delle tecniche di segmentazione, architettura U-Net e stato dell’arte nella segmentazione del seno in MRI 3D  
- **Dataset**: Duke Breast Cancer MRI – composizione, annotazioni, preprocessing e suddivisione in train/val/test :contentReference[oaicite:3]{index=3}  
- **Configurazione sperimentale**: gestione tramite file YAML (training, trasformazioni, modello, ottimizzazione, valutazione) e logging degli esperimenti :contentReference[oaicite:4]{index=4}  
- **Esperimenti**:  
  - evoluzione architetturale e tuning iperparametri  
  - confronto approccio 2D (SliceUNet) vs 3D (U-Net 3D)  
  - introduzione dell’Attention U-Net  
  - esperimento finale con post-processing morfologico (closing)  
- **Conclusioni**: risultati finali, implicazioni cliniche, possibili downstream task e prospettive future  

---

## ⚙️ Strumenti Utilizzati
- **Linguaggio**: Python  
- **Framework Deep Learning**: [PyTorch](https://pytorch.org/)  
- **Libreria Medical Imaging**: [MONAI](https://monai.io/)  
- **Ambiente**: Linux, GPU NVIDIA Tesla V100 / A100 (CUDA)  
- **Editor & Tooling**: VS Code, SSH per accesso remoto a GPU universitarie  

---

## 📊 Risultati Principali
- Pipeline completamente documentata e riproducibile tramite **script Python + configurazioni YAML**  
- Modello finale con **Dice score 0.9161** dopo post-processing  
- Benefici clinici attesi:
  - riduzione variabilità inter-operatore  
  - supporto alla refertazione veloce  
  - miglior stima della densità mammaria per valutazione del rischio oncologico  

---

## 📚 Riferimenti
- Dataset: [Duke Breast Cancer MRI](https://doi.org/10.7937/TCIA.2019.4VAFYFM9)  
- Oktay et al., *Attention U-Net: Learning Where to Look for the Pancreas*, arXiv:1804.03999  
- [MONAI Framework](https://monai.io/)  
- [PyTorch](https://pytorch.org/)  

---

## 👤 Autore
**Achille Cannavale**  
Corso di Laurea Magistrale in Ingegneria Informatica  
Università degli Studi di Cassino e del Lazio Meridionale  
Anno Accademico 2024/2025  

