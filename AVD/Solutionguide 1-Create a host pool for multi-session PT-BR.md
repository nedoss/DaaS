# Guia de solução 1: Criar um Host Pool para Multi-Session

Neste desafio, você criará pooled desktops ingressadas no Azure AD a partir de uma Imagem Multi-Session do Microsoft Windows 11. Após a implantação, você se conectará a esse host de sessão com o cliente de Área de Trabalho Remota nativo ou Web. 

## Desafio

Crie um hostpool Multi-Session do AVD ingressado no Microsoft Entra ID:
- Região: **Brasil South**
- Localização dos metadados: **EAST US**
- Ambiente de validação: **Não**
- Criar Session Host Configuration: **Não**
- Tipo de host pool: **Pooled**
- Família de computação: **Standard D2as v5**
- Escolha a Imagem da Galeria **Windows 11 Enterprise multi-session Version + Microsoft 365 Apps** 
- Domínio a ser ingressado: **Microsoft Entra ID (Enroll with Intune "No")**
- Registre desktop app group no **new workspace**
- Atribuir usuários
- Habilitar Microsoft Entra ID single sign on: **"Connections will use Microsoft Entra authentication to provide single sign-on."**.

Faça login no session host
- Faça login como usuário e verifique se o sistema operacional é uma sessão multi-session

Criar novos Remote Apps
- Implante o Microsoft Edge e o Word (ou qualquer outro) como aplicativos remotos

## Critério de Sucesso
1. Os Host Pools são criados e os Session Hosts são exibidos como disponíveis
2. Os usuários são atribuídos ao grupo de aplicativos apropriado do Host Pool
3. Capaz de mostrar as configurações do Host Pool definidas
4. As VMs são ingressadas no Microsoft Entra ID
5. Os usuários podem entrar na VM
6. Os aplicativos do Office podem ser acessados via RemoteApp

## 💡 Dicas 💡 
> **1.** Estamos implantando um Host Pool somente EntraID. **Nesse caso, você precisa definir direitos do IAM (RBAC) no nível do  resource group**. [Mais informações aqui](https://learn.microsoft.com/en-us/azure/virtual-desktop/azure-ad-joined-session-hosts#assign-user-access-to-host-pools)

- Atribua seus usuários a função de **Virtual Machine User Login para** que eles possam entrar nas VMs.
- Atribua aos administradores que precisam de privilégios administrativos locais a função de **Virtual Machine Administrator Login**.

> **2.** Se você estiver tentando acessar sua área de trabalho virtual de **Dispositivos Windows ou outros dispositivos que não estão conectados no mesmo Entra ID tenant**, adicione **targetisaadjoined:i:1** como uma propriedade RDP personalizada ao Host Pool. [Mais informações aqui](https://learn.microsoft.com/en-us/azure/virtual-desktop/deploy-azure-ad-joined-vm#access-azure-ad-joined-vms)

## Etapa 1 - Criar um novoPooled Host Pool e Workspace

1. Entre no [Portal do Azure](https://portal.azure.com/).

2. Pesquise **Azure Virtual Desktop** e selecione-a na lista.

3. Em Gerenciar, selecione **Host Pools** e selecione **+ Create**.
   
![Esta imagem mostra onde selecionar Host Pools em gerenciar e selecione adicionar para adicionar um novo Host Pool.](../Images/AVD/01-avdHostPool.png "Azure Virtual Desktop")

4. Na página Basics, consulte a captura de tela a seguir para preencher os campos obrigatórios. Selecione sua Assinatura, Resource Group e defina um nome de Hostpool. Como Localização, escolha **East US**. 

> **Info:** Isso afetará apenas os metadados. O local do datacenter para máquinas virtuais será exibido. 

Altere **Validation environment** para **No**.
Depois de concluído, selecione **Next: Session hosts**.

![Esta imagem mostra onde você inserirá as informações do Host Pool.](../Images/AVD/02-Hostpool_create_multisession_2.png "Criar página de Host Pool em pool")

5. Navegue até **Add Virtual Machines** na parte superior e selecione **Yes** para adicionar novas máquinas virtuais ao Host Pools do AVD.

Selecione seu **Resource group** e insira um **VM name prefix**, ele acrescentará automaticamente "-[número]" ao nome da VM. Siga os valores mostrados na imagem abaixo:

![Esta imagem mostra onde você inserirá as informações dos hosts da sessão do AVD.](../Images/AVD/02-Hostpool_create_sessionhosts_2.png)

![Esta imagem mostra onde você inserirá as informações dos hosts da sessão do AVD](../Images/AVD/02-Hostpool_create_sessionhosts_3.png)

Na última etapa, insira suas credenciais de administrador local, ignore a configuração de URL personalizada e clique em **Next: Workspace**.

![Esta imagem mostra onde você inserirá as informações dos hosts da sessão do AVD](../Images/AVD/02-Hostpool_create_sessionhosts_3.5.png)
 
7. Na página Espaço de trabalho, selecione **Yes** para registrar um novo grupo de aplicativos da área de trabalho. Selecione **Create new** e forneça um **Workspace name**. Selecione **OK** e **Review + create**.

![Esta imagem mostra como, na guia criar um espaço de trabalho do Host Pools, insira as informações necessárias.](../Images/AVD/02-hostpoolWorkspace.png "Criar uma guia de espaço de trabalho do Host Pool")

8. Na página Create a host pool, selecione **Review + Create** e na ultima tela em **Create**.

> **Observação**: se você estiver tentando acessar sua área de trabalho virtual de dispositivos Windows ou outros dispositivos que não estão conectados ao Azure AD, adicione **targetisaadjoined:i:1** como uma propriedade RDP personalizada ao Host Pool. [Mais informações aqui](https://learn.microsoft.com/en-us/azure/virtual-desktop/deploy-azure-ad-joined-vm#access-azure-ad-joined-vms)

**A Criação do Host Pool levará alguns minutos para acabar** Nos avise caso esteja demorando mais do que o esperado.

## Etapa 2 – Atribuir acesso de usuário ao Host Pool

Depois de criar seu Host Pool, **você deve atribuir aos usuários acesso ao Application Group dentro do Host Poll Criado**.  Recomendamos que você use grupos de usuários em vez de usuários individuais sempre que possível. Se você já atribuiu as permissões de Logon de Usuário de Máquina Virtual ou Logon de Administrador de Máquina Virtual no Desafio 1 no nível do Resource Group, essa tarefa não é mais necessária aqui. Se você ainda não fez isso ou se usa outro Resource Group, vá em frente e atribua essas funções aos usuários que devem ser capazes de fazer login nas áreas de trabalho em pool.

1. Atribua seus usuários ou grupos ao Application Group criado no exercício anterior. Ex: **Adicionar o Grupo GRP-T#-Users ao HP-AVD-T#-DAG**.

![Atribuir acesso ao Application Group](../Images/AVD/02-ApplicationGroupAssignment00.png)

2. Atribua aos usuários a função **Virtual Machine User Login** no **Resource Group** para que eles possam entrar nas VMs.

![Atribuir Virtual Machine User Login](<../Images/AVD/02-Virtual Machine User Login00.png>)

3. **opcional** Atribua aos administradores que precisam de privilégios administrativos locais a função de logon de administrador da máquina virtual.
Para conceder aos usuários acesso a VMs ingressadas no Azure AD, você deve [configurar atribuições de função para a VM](https://docs.microsoft.com/en-us/azure/active-directory/devices/howto-vm-sign-in-azure-ad-windows#configure-role-assignments-for-the-vm). 

**Em seguida, inicie o Windows App, atualize o espaço de trabalho do AVD e inicie o Desktop.**

## Etapa 3 - Criar Remote Apps

Para disponibilizar apenas um único aplicativo para o usuário, em vez da área de trabalho completa, para que apenas o aplicativo seja renderizado no dispositivo host do qual você está trabalhando no momento, você precisa criar um grupo de aplicativos remotos. Siga as orientações:

1. Navegue até a Área de Trabalho Virtual do Azure, selecione **Application Groups** e clique em **Create**.

![Esta imagem mostra como criar um novo grupo de aplicativos.](../Images/AVD/02-Hostpool_RemoteApp-1.png)

2. Na guia Basic, selecione seu **Resource Group**, seu AVD **Host Pools** e insira um **application group name**.

![Esta imagem mostra como criar um novo grupo de aplicativos.](../Images/AVD/02-Hostpool_RemoteApp-2.png)

Clique em **Next: Applications**

2. Em seguida, selecione o aplicativo que deseja adicionar ao seu application group e clique em **Add applications**.

![Esta imagem mostra como adicionar um aplicativo ao grupo de aplicativos.](../Images/AVD/03-Hostpool_RemoteApp-1.png)

Em seguida, selecione **Start menu** como a origem do aplicativo e **Microsoft Edge** (por exemplo) como o aplicativo .

![Esta imagem mostra como adicionar um aplicativo ao grupo de aplicativos.](../Images/AVD/03-Hostpool_RemoteApp-4.png)

Clique em **Review + add**, depois em **Add** e adicione outro aplicativo no menu Iniciar como a origem do aplicativo.

![Esta imagem mostra como adicionar um aplicativo ao grupo de aplicativos.](../Images/AVD/03-Hostpool_RemoteApp-5.png)

Clique em **Next: Assignments**.

3. Atribua o grupo Usuários do AVD ao Application group que deve ter acesso aos RemoteApps.

![Esta imagem mostra como atribuir um grupo de usuários ao grupo de aplicativos.](../Images/AVD/03-Hostpool_RemoteApp-2.png)

Clique em **Next: Workspace**

4. Alterne **Register Application Group** para **Yes** e clique em **Review + create**.

![Esta imagem mostra como atribuir um grupo de usuários ao grupo de aplicativos.](../Images/AVD/02-Hostpool_RemoteApp-2-1.png)

> Se outro application group no Host Pool do AVD já tiver sido registrado, esse grupo de aplicativos também será registrado no mesmo workspace.

**Em seguida, inicie o Windows App, atualize o espaço de trabalho do AVD e inicie os aplicativos remotos.**

> **Observação**: se você estiver tentando acessar sua área de trabalho virtual de dispositivos Windows ou outros dispositivos que não estão conectados ao Azure AD, adicione **targetisaadjoined:i:1** como uma propriedade RDP personalizada ao Host Pool. [Mais informações aqui](https://learn.microsoft.com/en-us/azure/virtual-desktop/deploy-azure-ad-joined-vm#access-azure-ad-joined-vms)


Desafio 2: **[Configurar Propriedades RDP](<Solutionguide 2-AVD-RDP-Properties - PT-BR.md>)**


## Recursos de Aprendizagem
- [Criar Hostpool da Área de Trabalho Virtual do Azure](https://learn.microsoft.com/en-us/azure/virtual-desktop/create-host-pools-azure-marketplace)
- [Gerenciar grupos de aplicativos para o portal da Área de Trabalho Virtual do Azure](https://learn.microsoft.com/en-us/azure/virtual-desktop/manage-app-groups)
- [Conectar-se à Área de Trabalho Virtual do Azure com o cliente da Área de Trabalho Remota para Windows](https://learn.microsoft.com/en-us/azure/virtual-desktop/users/connect-windows?tabs=subscribe#install-the-windows-desktop-client)

