A nested multiselect field for Laravel Nova. This package makes use of [vue3-treeselect](https://github.com/megafetis/vue3-treeselect)

To install this package use:

```bash
composer require marotura/nested-multiselect
```

This field extends the Nova Multiselect field. At the moment this is the most basic implementation.

```php
NestedMultiselect::make('Nested', 'nested')
    ->options([
        [
            'id' => 'a',
            'label' => 'a',
            'children' => [
                [
                'id' => 'aa',
                'label' => 'aa',
                ], [
                'id' => 'ab',
                'label' => 'ab',
                ],
            ]
        ], [
            'id' => 'b',
            'label' => 'b',
        ], [
            'id' => 'c',
            'label' => 'c',
        ]
    ])
```

Also make sure to cast your field to array of json

```php
protected $casts = [
        'nested' => 'json',
    ];
```