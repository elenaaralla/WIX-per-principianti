# Inizializzazioni 

**Quando si aggiunge un progetto di setup WIX v3 alla propria soluzione**

Le prime cose da fare sul file product.wxs sono 

-  modificare il tag MediaTemplate in modo che effettui l'embed cab (altrimenti il setup crea una file separato) 
        
        <MediaTemplate EmbedCab=”yes”/>

- indicare un **Name** al progetto e inserire il **Manufacturer** per evitare errori di compilazione

        <Product Id="*" Name="MYPROJECTNAME" Language="1033" Version="1.0.0.0" Manufacturer="MYMANUFACTORER" UpgradeCode="7555ea4a-10c8-46a0-b626-cc2f5a2755b4">

**Riferimenti**
 - [Creating a standard setup project using WiX Designer](https://www.add-in-express.com/creating-addins-blog/2014/04/08/creating-standard-install-package-wix-designer/)


# Property SET

Creare una property e valorizzarla a seconda del valore di un dato sul registro

**Leggo il valore dai registri**
    

        <Property Id="MY_REG_VALUE">
            <RegistrySearch Id="GPECAppPath"
                    Root="HKLM"
                    Key="SOFTWARE\WOW6432Node\[VENDOR]]\[APPNAME]"
                    Name="<key name>"
                    Type="raw" />
        </Property>


**Definisco la property con un valore di default (ad esempio se non trovo la chiave nei registri)**
    


        <Property Id="MY_PROPERTY" Value="DEF_VAL" />


**Setto un nuovo valore della property nel caso in cui la chiave dei registri (MY_REG_VALUE) esista**
    


        <SetProperty Id="MY_PROPERTY" After="AppSearch" Value="VAL" Sequence="first" >
          <![CDATA[MY_REG_VALUE]]>
        </SetProperty>


**Traduzione**

> cerco una chiave nei registri; definisco il valore della mia property e se trovo la chiave nei registri, setto il nuovo 
> valore, altrimenti la property rimane al valore di default! Se la chiave nei registri non c'è, MY_REG_VALUE non esiste e 
> quindi, dato che il SetProperty è condizionato all'esistenza di MY_REG_VALUE (<<![CDATA[MY_REG_VALUE]]>>), MY_PROPERTY
> rimane al valore di default.

**Riferimenti**

- [per definire correttamente una property](http://codebuckets.com/2016/07/23/conditional-deployment-with-wix/)
- [per cambiare il valore in funzione di una chiave di registro](https://stackoverflow.com/questions/26753632/wix-how-do-i-set-property-conditionally)    



# Shortcut

**Riferimenti**

- [per creare una shortcut dall'editor](https://stackoverflow.com/questions/3303962/visual-studio-deployment-project-create-shortcut-to-deployed-executable)


# Operating System Property Values

**Riferimenti**

- [per verificare la versione di Windows](https://docs.microsoft.com/it-it/windows/desktop/Msi/operating-system-property-values)


# File in use (problem)

**Il problema**

Dopo aver creato il progetto di setup e dopo aver fatto le dovute inizializzazioni

- vado sull'estensione FileSystem del Designer per WIX Toolset e aggiungo la cartella exe all'Application Folder; compilo->OK
- aggiungo un eseguibile nella cartella exe, perchè voglio che il setup copi nella cartella exe quell'eseguibile. compilo -> ERRORE (uno delle dll importate, perchè necessarie al mio eseguibile, risulta in uso)

**La soluzione**

- elimino la dll incriminata dall'elenco di quelle presenti nella cartella exe
- compilo -> OK
- riaggiungo la dll incriminata
- compilo -> OK

Mah! Il compilatore a sempre ragione, ma qualche volta mi vengono dei dubbi!


# Test materialize

- [test](./index.html)