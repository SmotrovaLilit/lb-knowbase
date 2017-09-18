# Ссылка в админку в амдинистративной панели

## Цель
Сделать так чтобы ссылка в админку в публичной части из административной панели вела на русскую версию сайта.

## Что надо сделать
- Повесить обработчики в init.php
```php
AddEventHandler("main", "OnBeforeProlog", [AdminUrlInPublicPageHandler::className(), 'onBeforeProlog']);
AddEventHandler("main", "OnAfterEpilog", [AdminUrlInPublicPageHandler::className(), 'onAfterEpilog']);
```

- Пример обработчиков

```php
use Bitrix\Main\Application;

/**
 * Класс делает так что ссылка в админку в админ. панели всегда ведет на русскую версию
 * Class AdminUrlInPublicPageHandler
 * @package OCS\Handler
 */
class AdminUrlInPublicPageHandler
{

    public static function className()
    {
        return get_called_class();
    }

    /**
     * Вначале страницы открываем буфер
     */
    public function onBeforeProlog()
    {
        if (ADMIN_SECTION === true) {
            return;
        }

        $request = Application::getInstance()->getContext()->getRequest();

        if ($request->isAjaxRequest()) {
            return;
        }

        ob_start();
    }

    /**
     * В конце страницы закрываем буфер и заменяем ссылку на переход в админку
     * @return bool
     */
    public function onAfterEpilog()
    {
        if (ADMIN_SECTION === true) {
            return true;
        }

        $request = Application::getInstance()->getContext()->getRequest();

        if ($request->isAjaxRequest()) {
            return true;
        }

        $content = ob_get_clean();

        preg_match("/<[Aa][\s]{1}([^>]*)id=\"bx-panel-admin-tab\"([^>]*)>/i", $content, $matches);

        if ($matches[1] || $matches[2]) {
            $currentText = $matches[1] ?: $matches[2];
            $resultText = str_replace('lang=en', 'lang=ru', $currentText);
            $content = str_replace($currentText, $resultText, $content);
        }

        echo $content;
        return true;
    }

}
```
