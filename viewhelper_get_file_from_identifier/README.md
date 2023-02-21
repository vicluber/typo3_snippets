# The largest heading
## The second largest heading
###### The smallest heading

A class that retrieves a file object by its identifier.

```
<?php

namespace Hebotek\DigitalesArchivStinatz\ViewHelpers;

use TYPO3Fluid\Fluid\Core\Rendering\RenderingContextInterface;
use TYPO3Fluid\Fluid\Core\ViewHelper\AbstractViewHelper;
use TYPO3Fluid\Fluid\Core\ViewHelper\Traits\CompileWithRenderStatic;
use TYPO3\CMS\Core\Resource\FileRepository;
use TYPO3\CMS\Core\Utility\GeneralUtility;

final class GetFileFormatViewHelper extends AbstractViewHelper
{
    use CompileWithRenderStatic;
    
    protected $escapeOutput = false;

    /**
     * @var FileRepository $fileRepository
     */
    protected ?FileRepository $fileRepository = null;

    public function initializeArguments()
    {
        // registerArgument($name, $type, $description, $required, $defaultValue, $escape)
       $this->registerArgument('fileUrl', 'string', 'The url', true);
    }

    public static function renderStatic(
       array $arguments,
       \Closure $renderChildrenClosure,
       RenderingContextInterface $renderingContext
    ) {
        $resourceFactory = \TYPO3\CMS\Core\Utility\GeneralUtility::makeInstance(\TYPO3\CMS\Core\Resource\ResourceFactory::class);
        $file = $resourceFactory->getFileObjectFromCombinedIdentifier('1:/'.$arguments['fileUrl']);
        return $file;
    }

}
```