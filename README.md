MANUAL
======

* Oscar Viveros
* Maestria en Ciencia de datos
* Procesamiento Nube
* Universidad Icesi - 2021

El propósito de este manual es explicar la configuración de diferentes componentes que ofrecen plataformas de servicios en la nube como lo son Google, Microsoft y DataBricks para responder preguntas relacionadas con el Covid-19 como son:

*	Cuantos casos existen a nivel mundial
*	Cuáles son las áreas o países más afectados
*	Identificar los puntos críticos en estados unidos
*	Índice de mortalidad.
  
La fuente de datos para resolver estas preguntas se toma del conjunto de datos públicos de BigQuery que contiene datos a nivel de país de series cronológicas diarias relacionadas con COVID-19 a nivel mundial.


#Prerrequisitos BigQuery

Migrar Datos Públicos cuenta de Google a una tabla del proyecto de BigQuery
----------------------------------------------------------------------------

Para poder migrarlos a un storage de Amazon o Azures los datos públicos deben estar migrados a una tabla del proyecto, para añadir los datos seguir los siguientes pasos



*	Crear un nuevo proyecto, seleccionar opción Select a Project y dar clic sobre el botón **NEW PROJECT**

    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura1.PNG)

*	Adicionar Nombre de proyecto y dar clic sobre el botón **CREATE**

    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura2.PNG)
 
*	Seleccionar el proyecto y dar clic sobre el servicio **BigQuery**
    
    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura3.PNG)

*	Para obtener los datos públicos, Dar clic sobre la opción **+ ADD DATA** y seleccionar **Explore public datasets**
    
    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura4.PNG)
 
*	En el campo de búsqueda digite “Covid-19 open data”, seleccionar el primer resultado
    
    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura5.PNG)

*	Dar clic en **VIEW DATA SET**

    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura6.PNG)
 
*	Los datos públicos quedan añadidos al proyecto

    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura7.PNG)

*	Expanda la lista **bigquery-public-data**, buscar la tabla de interés **covid19_open_data**

    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura8.PNG)
 
*	Para migrar la tabla al proyecto crear un Data set, para eso seleccionar el proyecto y dar clic sobre la opción **+ CREATE DATASET**
 
     ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura9.PNG)
 
*	Añadir un Dataset Id y dar clic sobre el botón **Create dataset**

     ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura10.PNG)


*	Para migrar los datos de la tabla externa dar clic sobre **+COMPOSE NEW QUERY**, copiar la sentencia SQL y dar clic en RUN
 
     ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura11.PNG)
     
      Sentencia SQL, esta sentencia copia todos los datos de la tabla externa a la tabla del proyecto

      CREATE TABLE DataSetCovid19.covid19_open_data
      OPTIONS(
        description="Covid 19 Data"
      ) AS
      SELECT  * FROM `bigquery-public-data.covid19_open_data.covid19_open_data`


*	Se hace un conteo de los registros para verificar que los datos se hayan migrado a la tabla del proyecto, colocar la siguiente sentencia sql y dar clic en RUN

    SELECT count(*) from DataSetCovid19.covid19_open_data


    
    

Generación de autenticación para acceder a los datos alojados en el proyecto de Google Cloud
--------------------------------------------------------------------------------------------

Para acceder a la información alojada en el proyecto de la cuenta de Google Cloud es necesario crear una autenticación OAuth 2, la cual genera atributos como la identificación del cliente (ClientId), la llave secreta del cliente (ClienteSecret), el token de actualización (AccesToken) y el token de actualización (RefreshToken), seguir los siguientes pasos para obtener los valores.

*	Ir a la opción Outh consent screen de **APIS & Services**
 
    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura12.PNG)
    
*	Seleccionar la opción External, dar clic en el botón **CREATE**
 
     ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura13.PNG)

*	En la opción **OAuth consent screen** ingresar el nombre de la aplicación, el correo de soporte y de desarrollo, dar click sobre el botón **SAVE AND CONTINUE**

    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura14.PNG)
 
*	En la opción de **Scopes**, dar clic sobre el botón **ADD OR REMOVE SCOPES**, seleccionar las opciones que se presentan y dar clic en el botón **UPDATE**; luego dar clic sobre el botón **SAVE AND CONTINUE**

    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura15.PNG)
 
*	En la opción Test users, dar clic sobre el botón **ADD USERS**, adicionar cuenta Google de usuario tester y dar clic sobre el botón **ADD**; luego dar clic sobre el botón **SAVE AND CONTINUE**. Luego en la opción Summary dar clic en el botón **BACK TO DASHBOARD**

    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura16.PNG)
    
*	Para obtener los datos de Cliente ID y Clien Secret, ir al menú de la consola de Google Cloud Platform seleccionar la opción **APIs & Services**, dar clic sobre **Credentials**

     ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura17.PNG)

*	Seleccionar la opción **+ CREATE CREDENTIALS**, dar clic sobre la opción **OAuth cliente ID**
 
     ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura18.PNG)
       
*	Seleccionar Web application como Application Type y colocar un nombre en el campo Name, en la opción de Authorized redirect URIs adicionar las siguientes URL

    * *	http://localhost:8080
    * *	https://developers.google.com/oauthplayground

    Dar clic sobre el botón **CREATE**
    
    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura19.PNG)
 
*	Anotar los valores de Client ID y Client Secret arrojados

     ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura20.PNG)

*	Para generar el token refresh el cual es necesario para conectarse con el dataSet de Bigquery se debe conectar a la siguiente URL**https://developers.google.com/oauthplayground/**

    En la pagina ir a la opción de configuración, seleccionar la opción **Use your OAuth credential**, agregar los valores de Client ID y Client Secret generada en los procesos anteriores. Dar click en opción **Close**

    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura21.PNG)
 

*	Busque el**API BigQuery API v2** y seleccionar la url**https://www.googleapis.com/auth/bigquery** , dar clic **Authorize Apis**

     ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura22.PNG)

*	Elija la cuenta asociada al aplicativo
 
    ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura23.PNG)

*	Dar clic sobre la opción Continue
 
   ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura24.PNG)
 
*	Dar clic sobre el botón Allow
 
 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura25.PNG)
 
 
*	Luego de la confirmación se genera un código de autorización, dar clic en **Exchange authorization code for tokens**, para generar los valores de **Refresh token** y **Access token**
 
 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura26.PNG)

*	Copiar los valores de Refresh token y Access token del resultado de la petición estos se utilizaran en la conexión desde Data Factory de Azure

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura27.PNG)



# Almacenamiento de datos en Block Storage de Azure

Seguir los siguientes pasos para pasar los datos de BigQuery a Block Storage de Azure utilizando Data Factory

Crear Resource Group
--------------------

Ingrese a Azure Cloud, Dar click sobre **Resource groups**
 
 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura28.PNG)
 
*	Dar clic en Crear sobre la opción **+ Create**

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura29.PNG)
 
*	Agregar nombre del grupo de recursos y seleccionar región, dar clic sobre el botón **Review + Create**.
  
  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura30.PNG)
 
*	Dar clic en el botón **Create**

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura31.PNG)
 

Crear block storage
-------------------
*	Seleccionar el resource group creado

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura32.PNG)

*	Dentro del recurso dar clic en **+ Add**

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura33.PNG)
 

*	Buscar Storage Account y seleccionar esa opción

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura34.PNG)

*	Dar click sobre el botón **Create**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura35.PNG)

*	Ingresar nombre de la cuenta de almacenamiento y dar clic sobre el botón **Review + create**
* *	Tipo de cuenta, siempre seleccionar la última versión
* *	Rendimiento estándar si requieren capacidades de velocidad mayores  utilizar premium
 
![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura36.PNG)


*	Dar clic en el botón **Create**

![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura37.PNG)
 
 
 Crear container, lugar donde se almacenarán los datos
------------------------------------------------------

*	Para crear un container asociado al Storage account, dar clic sobre el botón **Go to resource **
 
 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura38.PNG)

*	Dar clic sobre la opción **Containers**

![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura39.PNG)
 

*	Dar clic sobre la opción **+ Container**, y colocar el nombre del contenedor y dar clic sobre el botón **Create**

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura40.PNG)


Crear Data Factory (ETL)
------------------------

*	Ingresar al Resource group creado anteriormente, dar clic sobre la opción **+Add **

   ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura41.PNG)


*	Buscar Data Factory y seleccionarlo

   ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura42.PNG)

*	Dar Clic sobre el botón **Create**
 
 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura43.PNG)

*	Agregar nombre, seleccionar región y versión. Dar click sobre el botón **Review + créate**. Configurar Git Después

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura44.PNG)
 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura45.PNG)
 

*	Dar clic sobre el botón **Create**
 
![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura46.PNG)

*	Dar clic sobre el botón **Go to resource**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura47.PNG)

*	Para crear un trabajo, Dar clic sobre la opción **Author & Monitor**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura48.PNG)

*	Seleccionar **Copy Data**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura49.PNG)


*	Añadir propiedades, nombre de la tarea, descripción y seleccionar la opción **Run once now**, Dar clic sobre el botón **Next**

![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura50.PNG)
 

*	Dar clic sobre la opción **Create new connection**, buscar el componente Bigquery y dar clic sobre el botón **Create**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura51.PNG)


*	Para el nuevo Linked Server, adicionar en Project ID del proyecto de BigQuery donde está la tabla 

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura52.PNG)
 

* Adicionar también los valores Cliente ID, Client Secret y Refresh Token que se generaron en los prerrequisitos de Google dar clic en **Create**
 
![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura53.PNG)

*	Seleccionar el LinkedServer creado y dar clic sobre el botón **Next**

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura54.PNG)

*	Seleccionar la tabla asociada al DataSet, dar clic sobre el botón **Next**

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura55.PNG)


*	Dar clic sobre la opción  **+ Create new connection** y dar clic sobre el servicio **Azure Data Lake Storage** Gen2, dar clic en **Continue**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura56.PNG)

 
*	Seleccionar la suscripción y el storage account sobre el cual se van a descargar la información, Dar clic sobre el botón **Create**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura57.PNG) 

*	Seleccionar el nuevo LinkedServer y dar clic sobre el botón **Next**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura58.PNG) 

*	Dar click en Browse y seleccionar el container creado,colocar un nombre para el archivo a generar

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura59.PNG) 

*	Seleccionar el tipo de formato del archivo a generar, el delimitador de columna y el delimitador de fila, Dar clic en el botón **Next**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura60.PNG) 

*	En las opciones **Settings** y  **Summary** dar clic en el botón **Next**, esperar que se despliegue el trabajo y dar clic sobre el botón **Finish**

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura61.PNG) 

*	Después de terminado el trabajo, el archivo se puede encontrar en el Container designado para este trabajo

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura62.PNG) 




#Construcción de DataWharehouse en DataBricks

Crear Cluster
-------------

*	Inicialmente se debe crear un Cluster para esto se debe ir a la opción *New Clusters *

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura63.PNG) 


*	Colocar un nombre al clustes y dejar la versión runtime por defecto, dar clic sobre el botón **Create Cluster**, esperar que se despliegue el cluster
 
 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura64.PNG) 


Montar información de Azure Block Store y almacenar el resultado de las consultas en tablas
-------------------------------------------------------------------------------------------

*	Crear un Notebook, Dar clic sobre el modulo **WorkSpace**, seleccione la cuenta, seleccione **Create** y de clic sobre la opción **Notebook**

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura65.PNG) 

*	Ingrese un nombre para el Notebook a crear, seleccione el lenguaje Python y el cluster creado con anterioridad, Dar clic sobre el botón **Create**

![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura66.PNG) 
 

*	En el Notebook importat pandas

![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura67.PNG) 
 

*	Montar la información del Block Storage de Azure con la siguiente instrucción, reemplazar los elementos que se encuentren entre <> y ejecute la instrucción

    *   <container> = Nombre del contenedor creado en Azure y que contiene la información
    *   <storageaccount> = Cuenta de almacenamiento creada en Azure y que contiene el contenedor
    *	<Key> =llave que se debe sacar del storage account > Acces keys


![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura68.PNG) 

      dbutils.fs.mount(
        source = "wasbs://<container>@<storageaccount>.blob.core.windows.net",
        mount_point = "/mnt/covid19opendata",
        extra_configs = {"fs.azure.account.key.<storageaccount>.blob.core.windows.net":"<key>"})


  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura69.PNG) 


*  Validar la información montada con la siguiente instrucción, ejecutar la instrucción

   %fs ls /mnt/covid19opendata

   ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura70.PNG) 


*  Para verificar la estructura y información, generar un dataframe con la información montada, consultar los 10 primeros registros con las siguientes instrucciones

df = spark.read.csv("/mnt/covid19opendata/covid19opendata",header=True)

df.display(10)

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura71.PNG) 

*	Ejecute las siguientes instrucciones para crear las tablas que contienen la información a las preguntas solicitadas

    Se crea inicialmente una tabla temporal con todos los datos la cual se crea con la siguiente instrucción.

df.createOrReplaceTempView("covid19opendata")

 ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura72.PNG) 

Para responder las preguntas ¿Cuántos casos existen a nivel global?, ¿cuales son las áreas a países mas afectados?, Indice de mortalidad, se crea la tabla casos_x_pais con la siguiente instrucción
%sql   
DROP TABLE IF EXISTS casos_x_pais;

CREATE TABLE casos_x_pais AS 
SELECT country_name, sum(new_confirmed) tot_new_confirmed,  sum(new_deceased) tot_new_deceased
  FROM covid19opendata 
  WHERE aggregation_level =0
  GROUP BY country_name
  ORDER BY tot_new_confirmed Desc;

![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura73.PNG) 
  
Para confirmar la creación de la tabla y validar datos realizar consulta de los 10 primeros registros

![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura74.PNG) 

Para responder las preguntas ¿Identificar los puntos críticos en estados unidos?, se crea la tabla casos_us con la siguiente instrucción

%sql   
DROP TABLE IF EXISTS casos_us;

CREATE TABLE casos_us AS 
SELECT subregion1_name, sum(new_confirmed) tot_new_confirmed,  sum(new_deceased) tot_new_deceased
  FROM covid19opendata 
  WHERE aggregation_level =1
  AND country_name = 'United States of America'
  GROUP BY subregion1_name
  ORDER BY tot_new_confirmed Desc;

  ![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura75.PNG) 
  
Para confirmar la creación de la tabla y validar datos realizar consulta de los 10 primeros registros
 

![my_test_image](https://github.com/marardo/procesamiento_nube/raw/main/images/Captura76.PNG) 



