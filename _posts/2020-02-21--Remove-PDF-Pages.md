---
layout: post
title: "Remove PDF Pages"
date: 2020-02-21 00:52:04 +0000
categories: pdf pdfcpu powershell
tags: pdf pdfcpu powershell
excerpt: PowerShell can provide a simple GUI.  This can be used to remove pages from a PDF, such as a FAX cover sheet.
---


## gist: [Remove PDF Pages](https://gist.github.com/jftuga/5829db3b50428073a98b323f419e83ec)

**File:** Remove_PDF_Pages.ps1

{% highlight powershell linenos %}
<#
Remove_PDF_Pages.ps1
-John Taylor
Jan-7-2020

This is a simple, GUI frontend to pdfcpu.
https://github.com/pdfcpu/pdfcpu

The resulting PDF will have the filename appended with "--RemovedPages.pdf"
#>

Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

# Open File dialog box
Function Get-FileName($label) {
    [System.Reflection.Assembly]::LoadWithPartialName("System.windows.forms") | Out-Null
    $OpenFileDialog = New-Object System.Windows.Forms.OpenFileDialog
    $OpenFileDialog.initialDirectory = [Environment]::GetFolderPath('Desktop')
    $OpenFileDialog.filter = "Adobe (*.pdf)|*.pdf"
    $OpenFileDialog.ShowDialog() | Out-Null
    if ($OpenFileDialog.FileName -like '*--PagesRemoved*') {
        [System.Windows.Forms.MessageBox]::Show($OpenFileDialog.FileName + "`n`nThis file already has already had pages removed.", "Error")
        return
     }
    $label.Text = "File: " + $OpenFileDialog.FileName
    $OpenFileDialog.FileName
}

# GUI Controls
$form = New-Object System.Windows.Forms.Form
$form.Text = 'Remove PDF Pages'
$form.Size = New-Object System.Drawing.Size(500,300)
$form.StartPosition = 'CenterScreen'

if( $args.count -eq 0) {
    $SelectFileButton = New-Object System.Windows.Forms.Button
    $SelectFileButton.Location = New-Object System.Drawing.Point(10,20)
    $SelectFileButton.Size = New-Object System.Drawing.Size(95,20)
    $SelectFileButton.Text = 'Select PDF File'
    $global:pdfFileName = $null
    $SelectFileButton.add_Click({$global:pdfFileName = Get-FileName($FileSelectedLabel)})
    $form.Controls.Add($SelectFileButton)
} else {
    $global:pdfFileName = $args[0]
}

$FileSelectedLabel = New-Object System.Windows.Forms.Label
$FileSelectedLabel.Location = New-Object System.Drawing.Point(10,50)
$FileSelectedLabel.Size = New-Object System.Drawing.Size(490,40)
$FileSelectedLabel.Text = ''
if($args.count -eq 1) {
    $FileSelectedLabel.Text = 'File: ' + $global:pdfFileName
}
$form.Controls.Add($FileSelectedLabel)

$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,100)
$label.Size = New-Object System.Drawing.Size(280,20)
$label.Text = 'Which pages do you want to remove?'
$form.Controls.Add($label)

$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,120)
$label.Size = New-Object System.Drawing.Size(320,20)
$label.Text = 'Example: To remove pages 1-3 and page 5, input: 1-3,5'
$form.Controls.Add($label)

$textBox = New-Object System.Windows.Forms.TextBox
$textBox.Location = New-Object System.Drawing.Point(10,140)
$textBox.Size = New-Object System.Drawing.Size(260,20)
$form.Controls.Add($textBox)

$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(75,200)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = 'OK'
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)

$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,200)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = 'Cancel'
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)

$form.Topmost = $true
$form.Add_Shown({$textBox.Select()})
$result = $form.ShowDialog()

# When 'OK' is pressed
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $pagesToRemove = $textBox.Text
    if( $global:pdfFileName -notlike '*pdf') {
        [System.Windows.Forms.MessageBox]::Show($global:pdfFileName + "`n`nThis is not an Adobe PDF file.", "Error")
        exit
    }
    if( $pagesToRemove.Length -eq 0 ) {
        [System.Windows.Forms.MessageBox]::Show("You did not select any pages to be removed.","Error")
        exit
    }
    $newPdfFileName = $global:pdfFileName -replace ".pdf", "--PagesRemoved.pdf"
    pdfcpu.exe pages remove -pages $pagesToRemove "$global:pdfFileName" "$newPdfFileName"
    if( $? -eq $False ) {
        [System.Windows.Forms.MessageBox]::Show("An error occured when trying to remove pages.", "Error")
        return
    }
    # Show resulting file in File Explorer
    explorer.exe /select,"$newPdfFileName"
    [System.Windows.Forms.MessageBox]::Show("Created File:`n`n" + $newPdfFileName, "Remove PDF Pages")
}

# end of script
{% endhighlight %}
