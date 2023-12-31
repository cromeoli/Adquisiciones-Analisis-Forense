# Adquisición de una memoria USB con FTK, Guymager y dd

En este documento se resume brevemente el procedimiento para realizar una adquisición de una memoria USB con distintas herramientas, en esencia lo que haremos será crear una imagen de la memoria.

Estaremos trabajando con una memoria de 1 GB para facilitar y agilizar el proceso.

## FTK

En primer lugar voy a utilizar la herramienta FTK para realizar una imagen de la memoria USB. Para realizar la adquisición he hecho lo siguiente:

   1. Insertamos la memoria USB de la que queremos hacer la adquisición en un equipo con Windows y FTK instalados
   2. Abrimos la interfaz de usuario de FTK y seleccionamos *File/Create Disk Image*
   3. Seleccionamos la opción "*Physical Drive*" 
   4. Elegimos nuestra memoria USB
   5. Tendremos que seleccionar un directorio de destino para la imagen seleccionando "*Add...*"

   ![Alt text](img/image-6.jpeg)

   6. Para esta adquisición vamos a probar el formato .aff (Advanced Forensic Format).
   7. Le damos a *Start* y nos solicitará una serie de datos como Número de caso, de evidencia, descripción, nombre del examinador... Además de Destino de la imagen y nombre del archivo de imagen.
   8. Una vez esos datos estén rellenos, le damos a "*Finish*" y ya podremos darle a "*Start*" para comenzar la adquisición.

   ![Alt text](img/image-7.jpeg)

Tras unos minutos, tendremos creada la imagen en formato .aff y también se creará un archivo .txt con información sobre la adquisición. Se abrirá también una ventana de FTK con información adicional del análisis.

![Alt text](img/image-8.jpeg)

## Guymager

Para realizar una adquisición con Guymager, tendremos que en primer lugar conectar también nuestra memoria a la máquina con la que estamos trabajando y realizamos lo siguiente:

   1. Buscamos nuestra memoria USB y haciendo click derecho seleccionamos "Acquire image".

   ![guymager ui](img/image.png)

   2. Tendremos un menú similar al de FTK donde tendremos que rellenar datos de interés como el número de caso, de evidencia...etc. Los rellenaremos con los datos correspondientes. En el mismo menú, podremos seleccionar la ruta destino de la imagen junto a su nombre y el nombre del archivo de información.

   ![guymager metadata input](img/image-1.png)
   3. Ahora nos deja calcular el hash. Lo ideal es o bien calcular solo el SHA-256 o bien calcular el MD5 y el SHA-1. Vamos a probar la opción de calcular MD5 y SHA-1.

   ![Alt text](img/image-2.png)
   Podremos tras darle a *Start* y comenzará el proceso de adquisición

   ![Calculo de hash en guymager](img/image-3.png)

   Es interesante ver como guymager permite realizar varias operaciones simultáneas.

Comenzará el proceso de adquisición tras esto, y resultará en el archivo con extension .000 y un archivo con información del proceso con *.info* como extensión.

## dd

Para hacer una adquisición utilizando el comando `dd` de linux, haremos lo siguiente:

1. Conectamos por supuesto nuestra memoria USB a un equipo con una distribución de Linux
2. Lo localizamos en nuestro equipo, en nuestro caso es el `dev/sda1` y una vez lo tengamos, primero vamos a calcular el hash  con SHA1 de la memoria a copiar con el comando `shasum /dev/sda1`

   ![Obtencion de sha](img/1.png)

3. Una vez lo tengamos vamos a proceder a hacer la copia usando `dd` de la siguiente manera:

	```bash
   dd if=/dev/sda1 of=/home/user/adquisiciones/usb.dd bs =512 conv=noerror,sync
   ```

   ![Aplicación del comando dd](img/2.png)
4. Una vez tengamos la copia hecha, comprobamos su hash para ver si coincide (Debe hacerlo)

   ![Comprobación de hash](img/3.png)

Efectivamente vemos que coincide, por lo que con esto habríamos realizado nuestra adquisición utilizando el comando `dd`.
