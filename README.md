############################################
#Author:Lixiaosong
#Email:lixs@ourgame.com;lixiaosong8706@gmail.com
#For:设置SharePoint库文件夹安全组权限
#Version:1.0 2015年3月26日
##############################################
function Add-SPPermissionToSeGroup {
  Param(
      [Parameter(Mandatory=$True,Position=1)]
      [string]$SPWeb,
  
      [Parameter(Mandatory=$True,Position=2)]
      [string]$SPList,

      [Parameter(Mandatory=$True,Position=3)]
      [string]$ADUser,
   
     [Parameter(Mandatory=$True,Position=4)]
     [string]$SPFolder,

     [Parameter(Mandatory=$True,Position=5)]
     [string]$SPPermission
)

Add-PSSnapin Microsoft.SharePoint.PowerShell
#http://glproject/PMO/doc
$web = get-spweb "$SPWeb"


 function GrantUserpermission($userName)
 {
  [Microsoft.SharePoint.SPUserCollection]$spusers=[Microsoft.SharePoint.SPUserCollection]$web.SiteUsers
  [Microsoft.SharePoint.SPUser]$spuser=$spusers[$userName]
  $sproleass=new-object Microsoft.SharePoint.SPRoleAssignment([Microsoft.SharePoint.SPPrincipal]$spuser)
  $folder.BreakRoleInheritance("true")
  $sproleass.RoleDefinitionBindings.Add($web.RoleDefinitions["$SPPermission"])
  $folder.RoleAssignments.Add($sproleass);
  Write-Host "Permission provided for user ", $userName
 }
 $doclib=[Microsoft.SharePoint.SPDocumentLibrary]$web.Lists["$SPlist"]
 $foldercoll=$doclib.Folders;
 foreach($folder in $foldercoll)
 {
  Write-Host $folder.Name
  if($folder.Name.Equals("$SPFolder"))
  {
   GrantUserPermission("GLOBALLINK\$ADuser")
  }
 
 }
 Write-Host "Completed...."
 $web.Close()
}
