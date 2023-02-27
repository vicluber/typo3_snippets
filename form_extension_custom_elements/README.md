# Form create custom elements to add from the backend


## First define the yaml configurations for the tx_form extension in a yaml file:
```
plugin.tx_form {
    settings {
        yamlConfigurations {
            100 = EXT:your_sitepackage/Configuration/Forms/FrontendCustomFormSetup.yaml
        }
    }
}
module.tx_form {
    settings {
        yamlConfigurations {
            100 = EXT:your_sitepackage/Configuration/Forms/BackendCustomFormSetup.yaml
        }
    }
}
```
## Then we can start writingin these two files. For FrontendCustomFormSetup.yaml this will do it. You don't need ALL this definitions, this will work but right now is just a guide of how to do other stuffs.
```
TYPO3:
  CMS:
    Form:
      persistenceManager:
        allowedExtensionPaths:
          200: EXT:your_sitepackage/Configuration/Forms/
        allowSaveToExtensionPaths: false
        allowDeleteFromExtensionPaths: false

      prototypes:
        ### PROTOTYPE: STANDARD
        standard:
          formElementsDefinition:
            MultiSelectWithSystemCategory:
              __inheritances:
                10: 'TYPO3.CMS.Form.prototypes.standard.formElementsDefinition.MultiSelect'
              implementationClassName: 'Hebotek\DigitalesArchivStinatz\Domain\Model\FormElements\SystemCategoryOptions'
              renderingOptions:
                templateName: 'MultiSelect'
            Form:
              renderingOptions:
                templateRootPaths:
                    200: EXT:your_sitepackage/Resources/Private/Templates/Form/Frontend/
                partialRootPaths:
                    200: EXT:your_sitepackage/Resources/Private/Partials/Form/Frontend/
                layoutRootPaths:
                    200: EXT:your_sitepackage/Resources/Private/Layouts/Form/Frontend/
                translation:
                  translationFiles:
                    # index '10' is reserved for the default translation file.
                    20: 'EXT:your_sitepackage/Resources/Private/Language/locallang_forms_general.xlf'
                    30: 'EXT:your_sitepackage/Resources/Private/Language/locallang_forms_specific.xlf'
                    40: 'EXT:your_sitepackage/Resources/Private/Language/locallang_forms_custom.xlf'
          finishersDefinition:
            Confirmation:
              FormEngine:
                label: 'formEditor.element.AdvancedPassword.editor.confirmationLabel.predefinedDefaults'
                elements:
                  message:
                    label: 'formEditor.elements.Form.finisher.Confirmation.editor.header.label'
                    config:
                      type: 'input'
```


## Then we can write the next one, the definitions for backend.
```
TYPO3:
  CMS:
    Form:
      persistenceManager:
        allowedFileMounts:
          10: '1:/form_definitions/'
        allowSaveToExtensionPaths: false
        allowDeleteFromExtensionPaths: false
        allowedExtensionPaths:
          200: EXT:digitales_archiv_stinatz/Configuration/Forms/

      prototypes:
        standard:
          formEditor:
            translationFiles:
              10: 'EXT:form/Resources/Private/Language/Database.xlf'
            dynamicRequireJsModules:
              app: TYPO3/CMS/Form/Backend/FormEditor
              mediator: TYPO3/CMS/Form/Backend/FormEditor/Mediator
              viewModel: TYPO3/CMS/Form/Backend/FormEditor/ViewModel
            addInlineSettings: {  }
            maximumUndoSteps: 10
            stylesheets:
              200: 'EXT:form/Resources/Public/Css/form.css'
            formEditorFluidConfiguration:
              templatePathAndFilename: 'EXT:form/Resources/Private/Backend/Templates/FormEditor/InlineTemplates.html'
              partialRootPaths:
                10: 'EXT:form/Resources/Private/Backend/Partials/FormEditor/'
              layoutRootPaths:
                10: 'EXT:form/Resources/Private/Backend/Layouts/FormEditor/'

            formEditorPartials:
              FormElement-_ElementToolbar: Stage/_ElementToolbar
              FormElement-_UnknownElement: Stage/_UnknownElement
              Modal-InsertElements: Modals/InsertElements
              Modal-InsertPages: Modals/InsertPages
              Modal-ValidationErrors: Modals/ValidationErrors
              Inspector-FormElementHeaderEditor: Inspector/FormElementHeaderEditor
              Inspector-CollectionElementHeaderEditor: Inspector/CollectionElementHeaderEditor
              Inspector-TextEditor: Inspector/TextEditor
              Inspector-PropertyGridEditor: Inspector/PropertyGridEditor
              Inspector-SingleSelectEditor: Inspector/SingleSelectEditor
              Inspector-MultiSelectEditor: Inspector/MultiSelectEditor
              Inspector-GridColumnViewPortConfigurationEditor: Inspector/GridColumnViewPortConfigurationEditor
              Inspector-TextareaEditor: Inspector/TextareaEditor
              Inspector-RemoveElementEditor: Inspector/RemoveElementEditor
              Inspector-FinishersEditor: Inspector/FinishersEditor
              Inspector-ValidatorsEditor: Inspector/ValidatorsEditor
              Inspector-RequiredValidatorEditor: Inspector/RequiredValidatorEditor
              Inspector-CheckboxEditor: Inspector/CheckboxEditor
              Inspector-ValidationErrorMessageEditor: Inspector/ValidationErrorMessageEditor
              Inspector-Typo3WinBrowserEditor: Inspector/Typo3WinBrowserEditor
              Inspector-MaximumFileSizeEditor: Inspector/MaximumFileSizeEditor

            formElementPropertyValidatorsDefinition:
              NotEmpty:
                errorMessage: formEditor.formElementPropertyValidatorsDefinition.NotEmpty.label
              Integer:
                errorMessage: formEditor.formElementPropertyValidatorsDefinition.Integer.label
              NaiveEmail:
                errorMessage: formEditor.formElementPropertyValidatorsDefinition.NaiveEmail.label
              NaiveEmailOrEmpty:
                errorMessage: formEditor.formElementPropertyValidatorsDefinition.NaiveEmail.label
              FormElementIdentifierWithinCurlyBracesInclusive:
                errorMessage: formEditor.formElementPropertyValidatorsDefinition.FormElementIdentifierWithinCurlyBraces.label
              FormElementIdentifierWithinCurlyBracesExclusive:
                errorMessage: formEditor.formElementPropertyValidatorsDefinition.FormElementIdentifierWithinCurlyBraces.label
              FileSize:
                errorMessage: formEditor.formElementPropertyValidatorsDefinition.FileSize.label
              RFC3339FullDate:
                errorMessage: formEditor.formElementPropertyValidatorsDefinition.RFC3339FullDate.label

            formElementGroups:
              input:
                label: formEditor.formElementGroups.input.label
              html5:
                label: formEditor.formElementGroups.html5.label
              select:
                label: formEditor.formElementGroups.select.label
              custom:
                label: formEditor.formElementGroups.custom.label
              container:
                label: formEditor.formElementGroups.container.label
              page:
                label: formEditor.formElementGroups.page.label

          formEngine:
            translationFiles:
              10: 'EXT:form/Resources/Private/Language/Database.xlf'

      formManager:
        dynamicRequireJsModules:
          app: TYPO3/CMS/Form/Backend/FormManager
          viewModel: TYPO3/CMS/Form/Backend/FormManager/ViewModel
        stylesheets:
          100: 'EXT:form/Resources/Public/Css/form.css'
        translationFiles:
          10: 'EXT:form/Resources/Private/Language/Database.xlf'
        javaScriptTranslationFile: 'EXT:form/Resources/Private/Language/locallang_formManager_javascript.xlf'
        selectablePrototypesConfiguration:
          100:
            identifier: standard
            label: formManager.selectablePrototypesConfiguration.standard.label
            newFormTemplates:
              100:
                templatePath: 'EXT:form/Resources/Private/Backend/Templates/FormEditor/Yaml/NewForms/BlankForm.yaml'
                label: formManager.selectablePrototypesConfiguration.standard.newFormTemplates.blankForm.label
              200:
                templatePath: 'EXT:form/Resources/Private/Backend/Templates/FormEditor/Yaml/NewForms/SimpleContactForm.yaml'
                label: formManager.selectablePrototypesConfiguration.standard.newFormTemplates.simpleContactForm.label
        controller:
          deleteAction:
            errorTitle: formManagerController.deleteAction.error.title
            errorMessage: formManagerController.deleteAction.error.body
```

## And this is the file generated by the backend. (When you create a form in the backend, the extension writes this file and saves it in user_upload, but now because of our definition in FrontendCustomFormSetup.yaml in "allowedExtensionPaths" the extension will also look inside the folder /Form for offering us forms, and that way you can create and modify new forms programmatically. You can always use the backend as starting point)

```
renderingOptions:
  submitButtonLabel: Submit
type: Form
identifier: uploadRequestForm
label: 'Upload Request Form'
prototypeName: standard
renderables:
  -
    renderingOptions:
      previousButtonLabel: 'Previous step'
      nextButtonLabel: 'Next step'
    type: Page
    identifier: page-1
    label: Step
    renderables:
      -
        defaultValue: ''
        type: Text
        identifier: text-1
        label: Titel
      -
        defaultValue: ''
        type: Textarea
        identifier: textarea-1
        label: Kommentar
      -
        type: MultiSelectWithSystemCategory
        identifier: categories
        label: Kategorienauswahl
        properties:
          systemCategoryUid: 3
          fluidAdditionalAttributes:
            required: required
      -
        properties:
          saveToFileMount: '1:/user_upload/'
          allowedMimeTypes:
            - audio/mpeg
            - application/pdf
        type: FileUpload
        identifier: fileupload-1
        label: 'Einfaches Upload Feld'
      -
        defaultValue: ''
        validators:
          -
            identifier: EmailAddress
        type: Email
        identifier: email-1
        label: E-Mail-Adresse
```