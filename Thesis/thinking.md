Ho condotto questi esperimenti modificando gli iperparametri uno alla volta, cercando di migliorare sempre di più la metrica del dice score. Da un esperimento all'altro ho dovuto anche implementare in python dei sistemi di debugging, come ad esempio delle stampe delle dimensioni dei tensori o anche dei salvataggi delle immagini e le predizioni, tutto questo per capire se tutto era corretto.

Ora ti spiego un po' gli esperimenti che puoi trovare nel csv:

- Nel primo esperimento ho usato una configurazione base, con un resize per avere tutte le immagini della stessa dimensione, e una rete UNet così Unet(channels : [16, 32, 64, 128]; in_channels : 1; out_channels : 2; norm : BATCH; spatial_dims : 3; strides : [2,2,2])

- Nel secondo esperimento ho cambiato soltanto il learning rate di Adam, passando da 0.0005 -> 0.005, ottenendo un miglioramento del 1.17% sul test set

- Nel terzo esperimento ho aumentato la profondità della rete UNet, ottenendo un miglioramento sul validation set

- Per questo motivo nel quarto esperimento ho voluto rendere la rete ancora più profonda Unet(channels : [16, 32, 64, 128, 256]; in_channels : 1; out_channels : 2; norm : BATCH; spatial_dims : 3; strides : [2,2,2,2]), ottenendo così un miglioramento di circa 8% sul test set

- Nel quinto esperimento ho cambiato da Adam a AdamW con 0.0001, normalizzazione da Batch a Instance e una loss diversa, ovvero una combinazione tra Focal Loss e la Dice Loss con pesi diversi, ottenendo così un 86 sul validation set

- Nel sesto esperimento ho capito che avevo esagerato con un lr così basso, quindi l'ho portato a 0.001 avendo così un aumento del 3.11% sul test set

- Nel settimo esperimento ho introdotto un LR_scheduler, ovvero il LinearWarmupCosineAnnealingLR, che ha portato un piccolo miglioramento sul test

- Nell'ottavo esperimento ho aumentato la dimensione delle immagini da (384x384) a (512x512) ottenendo un altro piccolo aumento di performance

- Nel nono esperimento ho provato il padding al posto del resize, di 512, 512, 224, facendo crollare le performance

- Quindi nel decimo esperimento ho provato ad usare entrambe, ma ho ottenuto lo stesso performance basse


A questo punto ho deciso di avviare una serie di esperimenti seguendo un altro approccio, ovvero un approccio "2D", usando una roi_size di 512x512x1 e usando un RandSpatialCropSamplesd in combinazione con un padding. Per far funzionare questo tipo di approcci, ho dovuto creare un'architettura custom, in grado di poter avere ingressi 2D, chiamata SliceUNet. Poi ti darò il codice, ora mettine uno d'esempio.


- Nell'undicesimo ho quindi avuto 0,628 in test

- Per questo nel dodicesimo ho sostituito il padding con un resize di 512,512,0 salendo così a 0,7808

- Nel tredicesimo, Capendo che le performance erano più basse ho usato un approccio differente, usando delle dimensioni più grandi di roi_size = 512,512,16 e ritornando ad una Unet(channels : [32, 64, 128]; in_channels : 1; out_channels : 2; norm : INSTANCE; spatial_dims : 3; strides : [2,,2,2]) ottenendo 0,6413

- Nel quattordicesimo ho aumentato la profondità della rete Unet(channels : [16, 32, 64, 128]; in_channels : 1; out_channels : 2; norm : INSTANCE; spatial_dims : 3; strides : [2,2,2,2]) arrivando a 0,7376

- Nel quindicesimo, aumentando la profondità del resize a 224 sono arrivato a 0,7833

- Nel sedicesimo invece lasciando invariata la profondità nell'asse z, sono arrivato a 0,8668, ma spingendo a 150 epoche sono arrivato a 0,8863

A questo punto ho deciso di creare un sistema di valutazione più onesto, ovvero creando delle trasformazioni inverse in test, per quelle trasformazioni che modificavano la struttura delle label, così facendo la predizione finale si è potuta valutare con le label intonse.

- Nel diciassettesimo, con la seguente rete Unet(channels : [16, 32, 64]; in_channels : 1; out_channels : 2; norm : INSTANCE; spatial_dims : 3; strides : [2,2,1]), quindi ho avuto un calo di performance -> 0,6765


- Nel diciottesimo aumentando la profondità della rete ho ottenuto 0,8023

- Nel diciannovesimo ho sperimentato un'attention unet, spingendo fino a 150 epoche, ottenendo 0,8415

- Nel ventesimo ho riproposto l'esperimento 16, ma con il nuovo metodo di test, ottenendo 0,8567

- E infine nel 21 esimo esperimento spingendo a 200 epoche e avendo una rete profonda Unet(channels : [16, 32, 64, 128, 256]; in_channels : 1; out_channels : 2; norm : INSTANCE; spatial_dims : 3; strides : [2,2,2,1]) e una roi size così (512, 512, 8) sono arrivato a 0,9124