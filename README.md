# README

[![Travis Build Status](https://secure.travis-ci.org/egeloen/ivory-ordered-form.png?branch=master)](http://travis-ci.org/egeloen/ivory-ordered-form)
[![AppVeyor Build status](https://ci.appveyor.com/api/projects/status/k16kn70835m18rps/branch/master?svg=true)](https://ci.appveyor.com/project/egeloen/ivory-ordered-form/branch/master)
[![Code Coverage](https://scrutinizer-ci.com/g/egeloen/ivory-ordered-form/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/egeloen/ivory-ordered-form/?branch=master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/egeloen/ivory-ordered-form/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/egeloen/ivory-ordered-form/?branch=master)
[![Dependency Status](https://www.versioneye.com/php/egeloen:ordered-form/badge.svg)](https://www.versioneye.com/php/egeloen:ordered-form)

[![Latest Stable Version](https://poser.pugx.org/egeloen/ordered-form/v/stable.svg)](https://packagist.org/packages/egeloen/ordered-form)
[![Latest Unstable Version](https://poser.pugx.org/egeloen/ordered-form/v/unstable.svg)](https://packagist.org/packages/egeloen/ordered-form)
[![Total Downloads](https://poser.pugx.org/egeloen/ordered-form/downloads.svg)](https://packagist.org/packages/egeloen/ordered-form)
[![License](https://poser.pugx.org/egeloen/ordered-form/license.svg)](https://packagist.org/packages/egeloen/ordered-form)

The library allows to order your Symfony2 form fields by adding the position option. A position can either be first,
last or an associative array describing before and/or after field.

## Wandi Note

**Note: this is a Symfony 4 Fork**

If you want to use this Bundle with the Symfony Framework Standard Edition and use the `CreateForm` method in a `Controller` that extends `Symfony\Bundle\FrameworkBundle\Controller\AbstractController`, follow this quick guide:

```yaml
// config/services.yaml
Symfony\Component\Form\FormExtensionInterface: "@form.extension"
```

Before (classic case):
```php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;

class YourController extends AbstractController
{
    // [...]
    
    public function __construct(/* DI */)
    {
        // [...]
    }
    
    public function yourAction(Request $request): Response
    {
        $yourEntity = new Entity();
        
        $form = $this->createForm(YourType::class, $yourEntity, [
            // your options
        ]);
        $form->handleRequest($request);
        
         // [...]
    }
}
```

After:
```php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Form\FormExtensionInterface;
use Ivory\OrderedForm\Extension\OrderedExtension;
use Symfony\Component\Form\Forms;
use Ivory\OrderedForm\OrderedResolvedFormTypeFactory;

class YourController extends AbstractController
{
    /**
     * @var FormExtensionInterface
     */
    private $formExtension;
    
    public function __construct(FormExtensionInterface $formExtension)
    {
        $this->formExtension = $formExtension;
    }
    
    public function yourAction(Request $request): Response
    {
        $yourEntity = new Entity();
        
        $formFactory = Forms::createFormFactoryBuilder()
            ->setResolvedTypeFactory(new OrderedResolvedFormTypeFactory())
            ->addExtension($this->formExtension)
            ->addExtension(new OrderedExtension())
            ->getFormFactory();
        $form = $formFactory->create(YourType::class, $yourEntity, [
            // your options
        ]);
        $form->handleRequest($request);
        
         // [...]
    }
}
```


## Documentation

 1. [Installation](/doc/installation.md)
 2. [Usage](/doc/usage.md)
 3. [Known limitations](/doc/known_limitations.md)

## Testing

The library is fully unit tested by [PHPUnit](http://www.phpunit.de/) with a code coverage close to **100%**. To
execute the test suite, check the travis [configuration](/.travis.yml).

## Contribute

We love contributors! Ivory is an open source project. If you'd like to contribute, feel free to propose a PR! You
can follow the [CONTRIBUTING](/CONTRIBUTING.md) file which will explain you how to set up the project.

## License

The Ivory Ordered Form is under the MIT license. For the full copyright and license information, please read the
[LICENSE](/LICENSE) file that was distributed with this source code.
