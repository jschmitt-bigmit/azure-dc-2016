# Windows Server 2019 - Standalone Domain Controller Template


Ce Template ARM vous permet de déployer une image Windows Server 2019 et la préconfigurer afin d'y installer un rôle Controleur de domaine.

Description | Link | Visualize
--- | --- | ---
Déploiement Windows Server 2019 - Nouvelle Foret AD  | <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjschmitt-bigmit%2Fazure-dc-2019-supdevinci%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a> | <a href="http://armviz.io/#/load=https%3A%2F%2Fraw.githubusercontent.com%2Fjschmitt-bigmit%2Fazure-dc-2019-supdevinci%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://armviz.io/visualizebutton.png"/></a>

 # Déploiement:

 Paramètres du template:

* envName      : Préfixe qui va être repris pour le nommage des VM : Par défaut SUP.
* vNETAddress  : Préfixe réseau du réseau Virtuel, par défaut : 192.168.50
* userName     : Username Windows de la session Admin Locale
* userPassword : Mot de passe de la session Admin Locale
* domainName   : Nom de la forêt AD et du domaine qui vont être créé.

Modules déployés.

* Windows Server 2019 - Rôle AD DS
  * Network Security Group avec une règle de whitelist du port RDP (A REFINER)
  * Public Ip Address - Static
  * Script Powershell qui installe le rôle ADDS et le configure
  * Extension BGInfo 
  * Compte de stockage pour les diagnostics de la VM
* Virtual Network
  * 2 subnets: Application, Management
  * DNS Server : ADDS Server


Déploiement avec PowerShell :

```PowerShell
# --- Set resource group name and create
$ResourceGroupName = "pr-prod-rg"
New-AzureRmResourceGroup -Name $ResourceGroupName -Location "West Europe" -Force

# --- Deploy infrastructure
$DeploymentParameters = @{
    envName = "SUP"
    vNETAddress = "192.168.50.0"
    userName = "AdminDC"
    userPassword = "PasswordPourri"
    domainName = 'supdevinci.lan'

}

New-AzureRmResourceGroupDeployment -Name "deployment-01" -ResourceGroupName $ResourceGroupName -TemplateFile .\examples\example-linked-template.json @DeploymentParameters
```
