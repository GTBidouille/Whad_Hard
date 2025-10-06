Bonjour, j'ai commencé à regarder la partie sniffer 40 canaux pour que cela puisse être utilisé par Whad

au début j'étais parti sur l'idée de faire 40 dongles et de les coller sur un hub usb mais je n'ai pas trouvé de hub usb assez grand, donc je serai parti sur une cascade de hub usb. Il aurait fallu gérer les tensions puis je me suis dit que peut être on pouvait faire differement


je suis parti dans l'idée de passer par un microntrolleur qui pourrait parler via SPI au module rf52840 ( en passant par la partie DMA, soit en demander au module de me prévenir quand il a des données, soit en faisant une lecture réguliere)

après mes recherches, je penche pour un STM32 ( H7,F7 ou encore H5 car ils peuvent gérer 6 bus SPI different). Cela me permettrait de limiter le nombre de CS ( en gros je lis les modules 1 de chaque carte enfant), je peux me servir du DMA pour limiter l'utilisation des modules RF, je peux buffeuriser, la vitesse est suffisante pour gerer mes 40 modules ( 5 x 8 en fait) et du coup je peux donner un résultat des 40 canaux en une seule fois.

je pense que je dois pouvoir avoir un CS commun pour les cartes sans avoir à passer par une porte NAND par exemple, il faut juste que je fasse attention au pull-up ou pull-down 

concernant la carte, j'ai choisi  la NUCLEO-F767ZI (STM32F767ZI) car elle semble convenir au besoin pour un cout plus bas que les autres. Le STM32F767ZI peut revenir à 11€ piece et la NUCLEO-F767ZI on peut la trouver à 23€

pour la partie utilisation du rf52840, je vais me baser sur le pdf schematics and PCB ainsi que sur la partie datasheet. Le cout d'un rf5280 est entre 4 et 9 €

questions: 
	- est ce que l'on part sur des antennes PCB, céramiques ou filaire
	- je pense partir sur du polling régulier mais je pourrais gérer les flux via des INT ( peut se modifier sans avoir a toucher au PCB)


je vais mettre 

broches utilisées sur le F767ZI 


pour la partie SPI ( tous en full duplex master)
| bus SPI | Miso | Mosi | CLK |
|---|---|---|---|
| SPI1 | PA6 | PA7 | PA5 | 
| SPI2 | PC2 | PC1 | PB10 | 
| SPI3 | PC11 | PB2 | PC10 | 
| SPI4 | PE5 | PE6 | PE2 | 
| SPI5 | PF8 | PF9 | PF7 | 

pour la partie CS : 
- CS1:PF13
- CS2:PF14
- CS3:PF15
- CS4:PG0
- CS5:PG1
- CS6:PE7
- CS7:PE8
- CS8:PE9

pour la partie présence
- P1:PF0
- P2:PF1
- P3:PF2
- P4:PF3
- P5:PF4



pour chaque SPI, le DMA sera activé aussi bien en TX que RX. On peut choisir le mode normal ou circular, je pense que normal est plus adapté

sur le rf52840

p1.04 : Mosi
p1.02 : CLK
p1.06 : Miso
p1.09 : /CS
