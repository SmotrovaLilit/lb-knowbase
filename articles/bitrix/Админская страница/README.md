# Создание админской страницы. Админская таблица

## Создание админской страницы
- Создаем в папке local файл админской страницы к примеру  **/local/example_admin_page.php**
```php
<?php

require $_SERVER['DOCUMENT_ROOT'] . "/bitrix/modules/main/include/prolog_admin_before.php";

global $APPLICATION;

ob_start();

//здесь будет содержимое админской страницы

$content = ob_get_clean();
$request = \Bitrix\Main\Application::getInstance()->getContext()->getRequest();
if (!$request->get('isAjax')) {
    if ($request->get('popup')) {
        require($_SERVER["DOCUMENT_ROOT"]."/bitrix/modules/main/include/prolog_popup_admin.php");
    } else {
        require $_SERVER['DOCUMENT_ROOT'] . "/bitrix/modules/main/include/prolog_admin_after.php";
    }
}

echo $content;


if (!$request->get('isAjax')) {
    if ($request->get('popup')) {
        require($_SERVER["DOCUMENT_ROOT"]."/bitrix/modules/main/include/epilog_popup_admin.php");
    } else {
        require $_SERVER['DOCUMENT_ROOT'] . "/bitrix/modules/main/include/epilog_admin_before.php";
    }
}

$APPLICATION->SetTitle("Настройки платежной системы Uniteller");
require_once $_SERVER['DOCUMENT_ROOT'] . "/bitrix/modules/main/include/epilog_admin_after.php";

```
- Создаемм ссылку на этот файл в папке /bitrix/admin/ **/bitrix/admin/example_admin_page.php**
```php
<?php
require($_SERVER["DOCUMENT_ROOT"]."/local/admin/example_admin_page.php"); 
?>

```

## Создание админской таблицы
- Создаем компонент, в коде компонента  примерно следующее
```php
class AdminPageExample extends \CBitrixComponent
{
    const TABLE_ID = 'unical_identificator_page'; //уникальный идентификатор страницы

    /**
     * @var array
     */
    private $errors = [];

    /** @inheritdoc */
    public function executeComponent()
    {
        $this->arResult["TABLE_ID"] = self::TABLE_ID;
        $sort = new CAdminSorting($this->arResult["TABLE_ID"], "ID", 'desc');
        $admin_list = new CAdminList($this->arResult["TABLE_ID"], $sort);
        
        
        if($admin_list->EditAction()) //если нажали на редактирование строки таблицы
        {
            foreach($_REQUEST["FIELDS"] as $ID => $arFields)
            {
                $ID = intval($ID);

                if(!$admin_list->IsUpdated($ID))
                    continue;

                if(!$this->update($ID, $arFields))
                {
                    $admin_list->AddUpdateError("Не удалось изменить ", $ID);

                }
            }
        }
        $this->arResult['ADMIN_LIST'] = $admin_list;
        $this->arResult["ITEMS"] = $this->getItems();
        $this->includeComponentTemplate();
    }
    
    private function update($ID, $arFields) {
        //изменение строки таблицы 
    }
    
    public function getItems()
    {
        //формирование данных таблицы, сейчас тут заглушки
        $items = array();
        
        $items[] = array(
            "ID" => 1,
            "NAME" => 'название в первой строке',
            "DESCR" => 'описание в первой строке',
        );
        $items[] = array(
            "ID" => 2,
            "NAME" => 'название во второй строке',
            "DESCR" => 'описание во второй строке',
        );  
        
        return $items;
    }
}

```
- Шаблон компонента
```php
/** @var CAdminList $admin_list */
$admin_list = $arResult['ADMIN_LIST'];
$admin_list->GroupAction();
$admin_list->NavText($arResult['NAV_PRINT']);
//заголовки таблицы
$admin_list_headers = [
    [
        "id"       => 'ID',
        "content"  => 'ID',
        "default"  => true, //показывать ли колонку по умолчанию
    ],
    [
        "id"       => 'NAME',
        "content"  => 'Название',
        "default"  => true,
    ],
    [
        'id'        => 'DECSR',
        'content'   => 'Описание',
        'default'   => true,
    ],
];

$admin_list->AddHeaders($admin_list_headers);
$visible_columns = $admin_list->GetVisibleHeaderColumns(); //получить видимые колонки(беретя из настроек пользователя b_user_option)
foreach ($arResult['ITEMS'] as $item) {
    $row = &$admin_list->AddRow($item['ID']);
    foreach ($item as $key => $value) {
        if (in_array($key, $visible_columns)) {
            $row->AddField($key, $value, true); //последний параметр является ли поле редактируемым
        }
    }
}

$admin_list->CheckListMode();
$admin_list->DisplayList();

```

