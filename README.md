

# Property SET

Creare una property e valorizzarla a seconda del valore di un dato sul registro

--Leggo il valore dai registri--
    
    ```

    <Property Id="MY_REG_VALUE">
        <RegistrySearch Id="GPECAppPath"
                Root="HKLM"
                Key="SOFTWARE\WOW6432Node\[VENDOR]]\[APPNAME]"
                Name="<key name>"
                Type="raw" />
    </Property>
    ```

--Definisco la property con un valore di default (ad esempio se non trovo la chiave nei registri)--
    
    ```

    <Property Id="MY_PROPERTY" Value="DEF_VAL" />

    Setto un nuovo valore della property nel caso in cui la chiave dei registri (MY_REG_VALUE) esista
    <SetProperty Id="MY_PROPERTY" After="AppSearch" Value="VAL" Sequence="first" >
      <![CDATA[MY_REG_VALUE]]>
    </SetProperty>
    ```

--Traduzione--

>cerco una chiave nei registri; definisco il valore della mia property e se trovo la chiave nei registri, setto il nuovo >valore, altrimenti la property rimane al valore di default! Se la chiave nei registri non c'è, MY_REG_VALUE non esiste e >quindi, dato che il SetProperty è condizionato all'esistenza di MY_REG_VALUE (<![CDATA[MY_REG_VALUE]]>), MY_PROPERTY rimane >al valore di default.

--Riferimenti--

- [per definire correttamente una property](http://codebuckets.com/2016/07/23/conditional-deployment-with-wix/)
- [per cambiare il valore in funzione di una chiave di registro](https://stackoverflow.com/questions/26753632/wix-how-do-i-set-property-conditionally)    






