# Добавление пункта в админское меню
```php
AddEventHandler('main', 'OnBuildGlobalMenu', 'MenuItemsAdd');
    public function MenuItemsAdd(&$aGlobalMenu, &$aModuleMenu)
    {
        foreach ($aModuleMenu as & $moduleMenu) {
            if ($moduleMenu['section'] == 'GENERAL') {
                $moduleMenu['items'][] = array(
                    'text' => 'Заголовок пункта меню',
                    'url' => 'page.php', //название файла куда ведет меню(в папаке bitrix/admin/)
                    'title' => 'Заголовок пункта меню',

                );
            }
        }
    }
```
