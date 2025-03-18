# Guia de solução 1: Configurar propriedades RDP

## Desafio 

- Pooled (Desktop / Remote App) Host pool 
- Permitir vários monitores
- O dimensionamento inteligente deve estar ativado
- Negar câmera e microfone
- Negar copiar e colar, storage, unidade de rede e redirecionamento de impressora

## Critério de Sucesso

- Você não pode acessar unidades locais ou de rede redirecionadas do host da sessão
- Você não pode copiar e colar de seus dispositivos locais para a área de trabalho ou aplicativo remoto

## Etapa 1 - Configurar as propriedades RDP do Hostpool do AVD 

### Opção 1 via Portal do Azure

No pooled / multi-session Host pool (RemoteApp), navegue até **Propriedades RDP**.

! [Propriedade RDP](.. /.. /Imagens/SolutionGuide/AVD/03-RDPProperty_1.png)
 
Em seguida, configure as seguintes propriedades RDP.

Display settings:
 | RDP Property | Value |
 |---|---|
 | Multiple displays | **Enable multiple display support** |
 | Smart sizing | **The local window content will scale when resized** |

! [Propriedade RDP](.. /.. /Imagens/SolutionGuide/AVD/03-RDPProperty_2.png)

Device redirection:
 | RDP Property | Value |
 |---|---|
 | Microphone redirection | **Disable audio capture from the local device**  |
 | Camera redirection | **Don't redirect any cameras** |
 | Drive/storage redirection | **Don't redirect any drives**  |
 | Clipboard redirection | **Clipboard on local computer isn't available in the remote session** |
 | Printer Redirection | **The printers on the local computer are not available in the remote session** |
 | USB device redirection | **Don't redirect any USB devices** |

! [Propriedade RDP](.. /.. /Imagens/SolutionGuide/AVD/03-RDPProperty_3.png)

Em seguida, clique em **Save**.
 
### Opção 2 via Powershell

Para adicionar ou editar várias propriedades RDP personalizadas, execute os seguintes cmdlets do PowerShell fornecendo as propriedades RDP personalizadas como uma cadeia de caracteres separada por ponto-e-vírgula:

```
$properties="drivestoredirect:s:;audiomode:i:0;videoplaybackmode:i:1;redirectclipboard:i:0;redirectprinters:i:0;devicestoredirect:s:*;redirectcomports:i:1;redirectsmartcards:i:1;usbdevicestoredirect:s:;enablecredsspsupport:i:1;redirectwebauthn:i:1;use multimon:i:1;audiocapturemode:i:0;camerastoredirect:s:;smart sizing:i:1"

Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -CustomRdpProperty $properties
```

You can check to make sure the RDP property was added by running the following cmdlet:
```
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | format-list Name, CustomRdpProperty
```


## Recursos de Aprendizagem
- [Personalizar propriedades RDP](https://learn.microsoft.com/en-us/azure/virtual-desktop/customize-rdp-properties)
- [Propriedades RDP suportadas](https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/rdp-files)
