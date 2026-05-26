# RechnerArchitekturProjekt

ConvolutionTreeAdder:
Die Multiplikation der wird hier nicht iterativ wie auf dem Paper berechnet sondern über einen Addiererbaum der die Additionen teilweise Parallel im BSD format ausführt
Man braucht in diesem Fall genau 8 input werte für den Addiererbaum da 8 die wortbreite der einzelenen pixel 8 bit gross ist. daher kann man das effizient als Baum darstellen mit tiefe log2(8)
Nachteil: man muss Parallel Aus Rom Lesen können um die Addierer Baum Inputs gleichzeitig anlegen zu können.
Der Parallel lesbare Rom ist hier einfach durch einen 8 fach duplizierten einfach lesbaren Rom ungesetzt.

Usage: 
-pixelwerte w0 bis w8 setzen [0:255]
-output: BSD darstellung des Scalars mit dem Gaussfilter: 
[[1 2 1],
[2 4 2],
[1 2 1]]

TinyTapeoutAdder:
BSD Addierer dier TC zahlen mit BSD zahlen addiert und umliegende schaltung die es erlaub den Adder auf den entsprechenden pins die vom Tiny Tapeout Projekt breitgestellt werden zu betreiben.
Usage: 
uio[0]: bestimmt ob in register A(TC) oder Register B(BSD) der wert von ui[7:0] geschrieben wird 
uio[1]: enabled die register und die input werte zu laden
uio[2]: schreibt das ergebnis in output register zurück
uio[3]: selector für unteren 8 bit[7­:0] oder die oberen 8[15:8] bit des ergebnisses (bei den oberen 8 bit sind nur die ersten 2 stellen relevant)

-bsp add 01011111 (BSD) und 0111 (TC) (3+7)

1. TC input anlegen an ui[7:0] => 0000 0111
2. uio[1] => 1, uio[0] => 0 (load register A)
3. clk
4. BSD input anlegen an ui[7:0] => 01011111  
5. uio[1] => 1, uio[0] => 1 (load register B)
6. clk
8. uio[2] => 1 (ergebnis ausgeben der unteren 8 bit)
9. uio[2] => 1 ,uio[3] => 1 (ergebnis ausgeben der restilichen bit)
=> zusammmen : 10 11011110 (ergebnis genau 10 dezimal)


Convolution:
nahe an der Architektur vom Paper("A Multiplierless Architecture for Image Convolution in Memory") nur wird hier die Addition mit dem Akkumulator und den vorberechneten gewichten im BSD format durchgeführt.
Usage: 
-pixelwerte w0 bis w8 setzen [0:255]
-enabel aktivieren
-nach 8 clock cycles liegt im BSD format das entsprechende skalar mit den Gaussfilter an.


