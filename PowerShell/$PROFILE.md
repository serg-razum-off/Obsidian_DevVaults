# This is a placeholder for $PROFILE
2023-11-27, 10:30
```
# PROFILE file in the project contains copy of $PROFILE

#TODO: figure out why 'PS> & PROFILE' or 'PS> . PROFILE' do not reload profile when called from script: function rlp {& $PROFILE}

# ============================= SETTINGS 
Set-StrictMode -Version Latest # for all scrips from PWS Cookbook use Strict Mode -VErsion 3
$ErrorActionPreference = "Stop"
$PSDefaultParameterValues["Out-Default:OutVariable"] = "lstOut" #last result that was cw out;  $lstOut to see


# Setting my Aliases --> #Γ¥ù scriping aliases should be duplicated in Classes to have Aliacses activated in other environments
# Γ¡ÉΓ¡ÉΓ¡ÉΓ¡ÉΓ¡É ==> importance of functions; most imptt are marked 5 Stars

#region Functions-helpers
# ======================= COMMON HELPERS 

#  SAVE SETTINGS
function save-settings {
    # Backs up settings from respective VS Code keybindings and from PS $PROFILE to files in ProjectSettings
    # Γ¡ÉΓ¡ÉΓ¡ÉΓ¡É
    Get-Content 'C:\Users\Sergey\AppData\Roaming\Code\User\settings.json' > 'D:\Projects\PBI Tools\Power-BI-Version-Control-repository\ProjectSettings\Settings-Copy\vs_userSetings'
    Get-Content 'D:\Projects\PBI Tools\Power-BI-Version-Control-repository\.git\config' > 'D:\Projects\PBI Tools\Power-BI-Version-Control-repository\ProjectSettings\Settings-Copy\git_config'
    Get-Content 'C:\Users\Sergey\AppData\Roaming\Code\User\keybindings.json' > 'D:\Projects\PBI Tools\Power-BI-Version-Control-repository\ProjectSettings\Settings-Copy\vs_keybindings'
    Get-Content $PROFILE > 'D:\Projects\PBI Tools\Power-BI-Version-Control-repository\ProjectSettings\Settings-Copy\PS_PROFILE'
    Get-Content $PROFILE > 'C:\Users\Sergey\PROJECTS\LangCh\ProjectSettings\Settings-Copy\PS_PROFILE'
}
# ==================== COMMON HELPERS: end 
#endregion

#region File System Helpers
# ================== File System Helpers 
Set-Alias -Name 'cat' -Value Get-Content -Scope Global
Set-Alias -Name 'touch' -Value New-Item -Scope Global
function touchd { New-Item -Path ($args -Join '\') -Force } #creates file in the folder that don't exist
Set-Alias -Name 'grep' -Value Select-String -Scope Global
Set-Alias -Name 'rm' -Value Remove-Item -Scope Global
Set-Alias -Name 'inv' -Value Invoke-Item -Scope Global 
Set-Alias -Name 'testp' -Value Test-Path -Scope Global # checks if exists -- paths, files, variables
Set-Alias -Name 'prop' -Value Show-Object -Scope Global # requires PowerShellCookbook Module installed  Γ¡ÉΓ¡ÉΓ¡ÉΓ¡ÉΓ¡É
Set-Alias -Name 'grid' -Value Out-GridView -Scope Global 
Set-Alias -Name 'splt' -Value Split-Path -Scope Global 

function cw { Write-Host $args }
function grepr { Get-ChildItem -rec | grep @args } # from Holmes -->in  this implement it becomes a PWS ProjectSearch    Γ¡ÉΓ¡ÉΓ¡ÉΓ¡ÉΓ¡É

# show only files
function lsa { Get-ChildItem -Force @args } # including hidden
function lsf { Get-ChildItem -File @args } 
function lsd { Get-ChildItem -Directory @args } 
function lssymb ([string]$path = ".") { Get-ChildItem -Path $path | Where-Object { $_.LinkType -like 'Symb*Link' } } # get symb Links
function touchsymbL ([string]$linkPath = ".", [string]$targetPath) { New-Item -ItemType SymbolicLink -Path $linkPath -Target $targetPath }


function lse { 
    # list Errs in th session 
    try { 0..($Error.Count - 1) | ForEach-Object { "[$($_)]--> [$($Error[$_])]" } } catch { "No Errs yet... bad worker"; $error.clear() } 
}   

function lsed ([int]$ErrIdx) {
    # displays details about requested Err
    if ($null -ne $ErrIdx) { $Error[$ErrIdx] | Get-Error; break } Get-Error 
} 
function lsec { $error.clear() }

function shtd {     
    # closes all opened apps and shuts the PC down 
    param(
        [parameter (Mandatory = $true)]         [ ValidateSet('RestartPC', 'ShutDownPC')]            [string] $TypeOf 
    )
    if ($TypeOf -eq 'ShutDownPC') 
    {	(get-process | Where-Object { $_.mainwindowtitle -ne "" -and $_.processname -ne "powershell" } ) | stop-process; stop-computer -computername localhost -Force }
        
    Restart-Computer             
}

Set-Alias -Name rn -Value Rename-Item
Set-Alias -Name rename -Value Rename-Item
function renameblk ([string]$partNameFrom, [string]$partNameTo) { 
    # renames files in the folder Example: {Class_API.ps1, Class_AZUR.ps1, Class_GIT.ps1} --> {API.ps1, AZUR.ps1, GIT.ps1}
    lsf -path .\PowerShell-Scripts\ -filter $partNameFrom | ForEach-Object { 
        Rename-Item -path $_ -NewName (
            (split-path $_ -Leaf) -replace $partNameFrom, $partNameTo
        ) 
    } 
}
function Update-Powershell { Invoke-Expression "& { $(Invoke-RestMethod https://aka.ms/install-powershell.ps1) } -UseMSI" } # iex, irm
# ================== File System Helpers: end ============================
#endregion

#region Get Help Helpers
# =================== GET HELP helpers ===========================
Set-Alias -Name 'help' -Value Get-Help -Scope Global 
Set-Alias -Name 'h-gal' -Value Get-Alias -Scope Global # just gal -- get cmdlet for alias; gal -Definition -- <cmdlet Name> get all aliases for cmdlet
Set-Alias -Name 'ihist' -Value Invoke-History -Scope Global
Set-Alias -Name 'chist' -Value Clear-History -Scope Global
Set-Alias -Name 'hist' -Value Get-History -Scope Global
#  --> gmo is a standard alias for this Get-Module
Set-Alias -Name 'vrb' -Value Get-Verbs -Scope Global # Alias for function below                        
# Γ¡ÉΓ¡ÉΓ¡É
Set-Alias -Name 'pwSof' -Value Search-StackOverflow -Scope Global # Alias for function from Holmes  mdl; param -- seartch str related to PowerShell
function sof { Start-Process "https://stackoverflow.com/search?q=$($args -join ' ')" }
function pwSofLast ([string]$topic, [string]$numberOfAnswers) {
    # searches for last topics in SOF for any domain. Specify domain / topic with [str] param
    $url = "https://api.stackexchange.com/2.0/questions/unanswered" +
    "?order=desc&sort=activity&tagged=$($topic)&pagesize=$($numberOfAnswers)&site=stackoverflow"
    $result = Invoke-RestMethod $url
    $result.Items | ForEach-Object { $_.Title; $_.Link; "" }
}
# searches for installed module
function gemo ([string] $Name) { Get-Module -n $Name -ListAvailable } 

# gets paths from evn: (env: in powershell)
function genvr ([string] $serach) { ((Get-ChildItem env:) | Select-Object name, value | Where-Object { $_ -like "*$($serach)*" }) } # genv is occupied =(

# conda env
function genvc {
    
        ((conda info -e) | Where-Object { $_ -match '\*' }) 
} 
    
#gets all paths valid for modules
function genvm { $env:PSModulepath -split ';' }

function Get-NetHelp ([string] $PWSCmdletName) {
    # dnh
    # $PWSCmdletName -- name of PowerShell cmd-let for which you want to get .Net help
    ### When the Get-Member cmdlet doesnΓÇÖt provide the information you need, the Microsoft documentation for a type is a great alternative. [Holmes, 128]
    # Γ¡É

    if ($PWSCmdletName -eq "") {
        Start-Process "https://learn.microsoft.com/en-us/dotnet/"
        exit
    }
    $PWSCmdletName = [scriptblock]::Create($PWSCmdletName).Invoke()[0].GetType().toString()
    Start-Process "https://learn.microsoft.com/en-us/search/?terms=$PWSCmdletName&scope=.NET"
}

function Get-CommandForContext($context) {
    # [System.ComponentModel.Description("Context=Website")] --> tags other functions for this Fn could categorize them Holmes [328]
    Get-Command -CommandType Function |
    Where-Object { $_.ScriptBlock.Attributes |
        Where-Object { $_.Description -eq "Context=$context" } }
}
function Get-Verbs { get-verb | clip && np } # lists correct verbs in NotePad++. NP is fn in this file

# =================== GET HELP helpers: end ========================
#endregion

#region DataProcess
# =================== DATA PROCESS helpers: start ========================
function csv2dt ($scv) {
    # returns DataTable Γ¡ÉΓ¡ÉΓ¡ÉΓ¡ÉΓ¡É
    # converts content of csv file into a System.Data.DataTable; $csv is object[] that should be derived by 
    #       $csv = Get-Content '.\Wheat Data-All Years.csv' -Force
    #       ConvertTo-Csv, etc
    # --------------- DataTable ---------------
    $columns = ($csv | Select-Object -First 1).split(',')
    $data = $csv | Select-Object -Skip 1	
    $sampleData = $data[0] -split ','

    $dt = New-Object System.Data.DataTable
    # $dt.Columns.AddRange($columns) # --> this adds all columns in one bulk but all datatypes are [stting]

    $columns | ForEach-Object -Begin { $i = 0 } -Process { 
        if ($sampleData[$i] -match "[0-9]+.?[0-9]?") { 
            [void]$dt.Columns.Add($_, [double]) 
        }
        else { 
            [void]$dt.Columns.Add($_, [string]) 
        } $i++ 
    }
    # $dt.Columns | ft
    $data | ForEach-Object { [void]$dt.Rows.Add($_.split(',')) }

    return , $dt; # explanation of return synx: https://stackoverflow.com/questions/35491390/powershell-function-will-not-return-datatable 
    
}
# =================== DATA PROCESS helpers: end ========================
#endregion

#region StartersFunctions
#------------------------- Starters helpers 
Set-Alias -Name getm -Value Get-StartApps
# Γ¡ÉΓ¡ÉΓ¡É np --> launches notepadd++
function np { Start-Process notepad++ $($args -join ' ') } 
Set-Alias -Name 'dnh' -Value Get-NetHelp -Scope Global # fn in this file                                                                  
function startm ([string] $AppName) { Search-StartMenu $AppName | invoke-item }   # Γ¡ÉΓ¡ÉΓ¡É requires PowerShellCookbook Module installed 
function run {
    [CmdletBinding()]
    param (
        [Parameter(
            ValueFromPipeline = $true,
            ValueFromPipelineByPropertyName = $true
        )]
        [Alias("FullName")]
        [String]$Path
    )

    process {
        Start-Process $Path
    }
}
function gt { Start-Process "https://translate.google.com/?sl=en&tl=uk&text=$($args -join ' ')&op=translate" } 
function dp { Start-Process "https://www.deepl.com/translator#en/uk/$($args -join ' ')" } 
function gs { Start-Process "https://www.google.com/search?q=$($args -join ' ')" }
function li { Start-Process "https://www.linkedin.com/in/sergiy-razumov-33670b131/" }
function gh { Start-Process "https://www.linkedin.com/in/sergiy-razumov-33670b131/" }

function gpt {
    <# chat gpt access  #>
    $param = $args -join ' '  
    switch ($param) {
        { $param -match '(emp|maha)' } { Start-Process "https://chat.openai.com/chat/924e36d4-640e-4ca5-bc83-b5a8afb90400"; break }
        { $param -match '(engl)' } { Start-Process "https://chat.openai.com/chat/fcefb111-8669-4323-9a34-c6f164c1569c"; break }
        { $param -match '(pws|powershell).arr' } { Start-Process "https://chat.openai.com/chat/abb9188c-cbef-4070-b2b7-a7f2a4ff6bc6"; break }
        { $param -match '(pws|powershell).ref' } { Start-Process "https://chat.openai.com/chat/a02d2d80-01ae-4397-bec9-cc2f51bb02a7"; break }
        { $param -match '(pws|powershell).(swi|swt)' } { Start-Process "https://chat.openai.com/chat/a02d2d80-01ae-4397-bec9-cc2f51bb02a7"; break }
        { $param -match '(pws|powershell).(misc|oth)' } { Start-Process "https://chat.openai.com/chat/7f47bb34-eb00-48f7-b483-58bf4901a79f"; break }
        { $param -match '(?i)(C#|json|jsn)' } { Start-Process "https://chat.openai.com/chat/6a1d1e1d-af62-41d8-bfb5-e91daedcfef5"; break }
        { $param -match '(git|misc)' } { Start-Process "https://chat.openai.com/chat/e898b56a-2ae0-4397-849a-5156252d82c3"; break }
        { $param -match '(vba|excel|xls)' } { Start-Process "https://chat.openai.com/chat/12dd2484-e369-4af7-bab9-3a6ca74bf1ae"; break }
        Default { Start-Process "https://chat.openai.com/chat"; break }
    }
     
}
function perpl { Start-Process "https://www.perplexity.ai/" } #ChatGPT analogue
function jsonv {
    param(
        [Parameter(Mandatory = $false)]
        [ValidateSet("tree", "grid", "ext")]
        [string]$TypeOfSite = "ext"
    )
    switch ($TypeOfSite) {
        "tree" { Start-Process "https://jsonformatter.org/json-viewer" }
        "grid" { Start-Process "https://jsongrid.com/json-formatter" }
        "ext" { Start-Process "https://jsonhero.io/" }
        Default { <# tree view is default #> }
    }
}
function dxg 
#keep in mind structure of the DaxGuide site: /group/section. Groups are dt: datatypes; op: operators; st: statements; blank: functions
{ Start-Process "https://dax.guide/$($args -join '/')" } 

function pbihotkeys { Start-Process "https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-accessibility-keyboard-shortcuts#on-visual" }

# --- this block is for CMTR Project 
# -----------------------------------

# starters ---------->
function shP4Automation { Start-Process "https://findepumodoutlook.sharepoint.com/sites/-773/Shared%20Documents/Forms/AllItems.aspx?newTargetListUrl=%2Fsites%2F%2D773%2FShared%20Documents&viewpath=%2Fsites%2F%2D773%2FShared%20Documents%2FForms%2FAllItems%2Easpx&id=%2Fsites%2F%2D773%2FShared%20Documents%2FGeneral%2F4Automation&viewid=cc0d12fb%2D3579%2D434f%2Daeaf%2D38d3e4af6e41" }
function shPlVoliaTeam { Start-Process "https://findepumodoutlook.sharepoint.com/sites/-773/Lists/List/AllItems.aspx" }
function powerAJobs { Start-Process "https://make.powerautomate.com/environments/Default-43a7b066-fe01-43db-b0ea-a9c1f1a18fc5/flows" }
function powerAConns { Start-Process "https://make.powerautomate.com/environments/Default-43a7b066-fe01-43db-b0ea-a9c1f1a18fc5/connections?status=error" }
function wsCMTRtoolkit { Start-Process "https://app.powerbi.com/groups/b45a52f9-02d5-4cdd-976f-ae1aa8fcb6dd/list?experience=power-bi" }
function wsVoliaProject { Start-Process "https://app.powerbi.com/groups/a806905c-57ba-4f13-87f3-97675cbd179f/list?experience=power-bi" }
function wsCMTRtopReports { Start-Process "https://app.powerbi.com/groups/59cc1355-9e31-4717-8674-f2e6566629d4/list?experience=power-bi" }
function wsAppVoliaProject { Start-Process "https://app.powerbi.com/groups/a806905c-57ba-4f13-87f3-97675cbd179f/apps/build/setup?experience=power-bi" }
function wsAppCMTRtopReports { Start-Process "https://app.powerbi.com/groups/59cc1355-9e31-4717-8674-f2e6566629d4/apps/build/setup?experience=power-bi" }
function wsJiraReports { Start-Process "https://app.powerbi.com/groups/25ccd0bd-62c4-4d76-a5ea-03508570d27f/list?experience=power-bi" }
function dflCMTRdataflows { Start-Process "https://app.powerbi.com/groups/a7d05c89-3d30-4a91-b35a-759548064d24/list?experience=power-bi" }
function dflVoliaTeam { Start-Process "https://app.powerbi.com/groups/46c1486b-e09b-4bf1-a8d2-14f35387dc95/list?experience=power-bi" }

# updaters ---------->
function updPSprofile { Get-Content $profile > 'C:\Users\RazumovSergii\OneDrive - UMOD Directory\Documents\PowerShell\Microsoft.PowerShell_profile.ps1' }



# functions ---------->
function cpOver {
    # TODO: SR[2023-11-27]: this has to be updated ΓÇö new param  -FileType ["json", "pbiv", "pbicmtr"], depending of which the path is different
    [CmdletBinding()]
    param (
        [Parameter(
            Position = 0,
            ValueFromPipeline = $true,
            ValueFromPipelineByPropertyName = $true
        )]
        [Alias("FullName")]
        [String]$SourcePath,
    
        [Parameter(
            Position = 1,
            Mandatory = $true
        )]
        [String]$DestinationPath
    )
    
    process {
        Copy-Item -Path $SourcePath -Destination $DestinationPath -Force
    }
}

# --- end block

#endregion

#region fun
#-------------------------- fun 
function Start-tekken3 { Start-Process "https://www.retrogames.cc/psx-games/tekken-3.html" }

function Start-MK3 { Start-Process "https://www.retrogames.cc/psx-games/mortal-kombat-3.html" }
# function hdtv { Start-Process "https://hdtoday.tv/search/$($args -join '-')" }
function hd { Start-Process "https://hdtoday.tv/search/$($args -join '-')" }
function math { Start-Process "https://poki.com/en/g/arithmetica" }
function yt { 
    $param = $args -join ' ';
    switch ($param) {
        { $param.Length -eq 0 } { $lnk = "https://www.youtube.com/" }
        { $param -match 'hist' } { $lnk = "https://www.youtube.com/feed/history" }
        { $param -match '(down|dwn|dw)' } { $lnk = "https://www.youtube.com/feed/downloads" }
        Default { $lnk = "https://www.youtube.com/results?search_query=$($param)" }
    }	
    Start-Process $lnk 
}
#endregion


# -------------------------- SETTING UP REQUIRED MODULES 
# requiref modules are here. Yes to [A]lsf  on install request 
# more on Pester https://pester.dev/docs/introduction/installation 
$modulesListToInstall = @(
    'ImportExcel',
    'PowerShellCookbook',
    #  'Pester',
    'MicrosoftTeams')
$modulesListToInstall | ForEach-Object {
    if (    -not (Get-Module $_ -ListAvailable)   ) { Install-Module -Name $_ -AllowClobber }
}

#region PROMPT
#  PROMPT -- function to create Prompt with 4-areas of information 
#       1. Git info + and - to main branch. "main" is hardcoded name 
#       2. Shortened location on Drive
#       3. Time, used for previous cmd Run
#       4. Current number of command. Can B used in "hist" calls
function prompt {
    ##############################################################################
    ##
    ## From PowerShell Cookbook (O'Reilly) --> was taken as base and severely reworked: SR [2022-11-01]
    ## by Lee Holmes (http://www.leeholmes.com/guide)
    ##
    ##############################################################################
    # Γ¡ÉΓ¡ÉΓ¡ÉΓ¡É
    $historyItem = Get-History -Count 1
    
    if ($null -ne $historyItem) {
        $prevId = $historyItem.Id
        $id = $historyItem.Id + 1
        
    }	
    else {
        $prevId = 0
        $id = 1
        
    }    
    ###################################### GIT 
    
    # SR: core solution for cmm diff based on 
    #   https://stackoverflow.com/questions/20433867/git-ahead-behind-info-between-master-and-branch
    #   https://stackoverflow.com/questions/2180270/check-if-current-directory-is-a-git-repository
    $isGitDir = git rev-parse --is-inside-work-tree 2>$null
    if ($null -ne $isGitDir) {
        $currBr = git branch --show-current
        
        try {
            $mainBrName = ([regex]::match((git symbolic-ref refs/remotes/origin/HEAD), "/m\w+").value).Substring(1)
            # fixing HEAD that is not a symbolic ref Err:
            #   git symbolic-ref refs/remotes/origin/HEAD refs/remotes/origin/main
        }
        # catch [System.Management.Automation.MethodInvocationException] {
        #     $mainBrName = "lc-" + ( git branch --show-current )
        # }
        catch {
            $mainBrName = "#-Err-Id-#"
        }

        # difference from origin/main
        try {
            $compMain = git rev-list --left-right --count origin/$mainBrName...origin/$currBr
            $mainBrAhead = $compMain[1] -eq [byte][char]9  ? ( $compMain[0] ) : ( $compMain[0] + $compMain[1] )
            if ($compMain.Length -le 3) {
                $currBrAhead = $compMain[2]            
            }
            else {
                $currBrAhead = $compMain[2] + $compMain[3]
            }
        }
        catch {
            ## 1 identified: currBranch was created, but not yet pushed to ORIGIN
            $mainBrAhead = '-'
            $currBrAhead = '-'
        }        
        
        Write-Host -NoNewline -BackgroundColor Green -ForegroundColor Black `
            "[$(git branch --show-current)]┬▒($($currBrAhead);$($mainBrAhead))>>"
    }
    else {
        Write-Host -NoNewline -BackgroundColor Green -ForegroundColor Black "[~not a GIT~]>>"
    }
    
    ###################################### LOCA 
    $pathStart = Get-Location | Split-Path -Qualifier
    $pathEnd = Get-Location | Resolve-Path -Relative
    $pathEndCombined = (( grep -Inp $pathEnd -Patt '(-| |\\|_).' -A | ForEach-Object { $_.matches.value } ) -join '').trim()
    
    ################################## EXEC Time 
    Write-Host -NoNewline -BackgroundColor DarkBlue -ForegroundColor White `
        "$($pathStart)" "..$($pathEndCombined) >>"     
    Write-Host -NoNewLine -BackgroundColor Yellow -ForegroundColor Black " PS:$prevId->"
    # identifying running time of the last command:
    if ($historyItem) {
        $ms = $((
	        (Get-History -Count 1).Duration 
            ).TotalMilliseconds / 1000 )
    }
    else {
        $ms = 0
    }
    Write-Host -NoNewLine -BackgroundColor Yellow -ForegroundColor Black "[$([math]::Round($ms,3))s]>>"
    Write-Host -NoNewLine -BackgroundColor DarkRed -ForegroundColor White " PS:$id >"
    break # to stop terminal line from do some automatic scripting
}
#endregion


```