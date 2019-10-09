# Ansible_Windows
Configuración para desplegar instrucciones sobre sistemas operativos Windows (PC y Server)


Requisitos del host 
Para que Ansible se comunique con un host de Windows y use módulos de Windows, el host de Windows debe cumplir los siguientes requisitos:

Las versiones de Windows admitidas por Ansible generalmente coinciden con las que están bajo el soporte actual y extendido de Microsoft. Los sistemas operativos de escritorio compatibles incluyen Windows 7, 8.1 y 10, y los sistemas operativos de servidor compatibles son Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016 y 2019.
Ansible requiere PowerShell 3.0 o posterior y al menos .NET 4.0 para instalarse en el host de Windows.
Se debe crear y activar un oyente WinRM. Más detalles para esto se pueden encontrar a continuación.

----------------------------------------------------------------------------------------------------------------

1. Debemos asegurarnos de disponer una autenticación con WinRM con el nodo. Para esto abrimos una consola shell en el equipo windows y ejecutamos los siguientes comando:

    $url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
    $file = "$env:temp\ConfigureRemotingForAnsible.ps1"

    (New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)

    powershell.exe -ExecutionPolicy ByPass -File $file

Nota

El script ConfigureRemotingForAnsible.ps1 está destinado únicamente a fines de capacitación y desarrollo y no debe usarse en un entorno de producción, ya que habilita configuraciones (como la Basicautenticación) que pueden ser inherentemente inseguras.

WinRM Listener 
Los servicios WinRM escuchan las solicitudes en uno o más puertos. Cada uno de estos puertos debe tener un oyente creado y configurado.

Para ver los escuchas actuales que se ejecutan en el servicio WinRM, ejecute el siguiente comando:

winrm enumerate winrm/config/Listener

2. Ingresamos al servidor ansible y a la carpeta donde se encuentran los playbooks a ejecutar:

    cd /user/Ansible_Windows_ELK_Agent\windows

3. Ejecutamos los playbooks de la siguiente manera

    ansible-playbook -i hosts -b -vvvv beats-v1-windows.yml --extra-vars all

Si se han ecriptado las variables debe ejecutarlo de la siguiente manera 

    ansible-playbook -i hosts -b -vvvv beats-v1-windows.yml --extra-vars all --ask-vault-pass