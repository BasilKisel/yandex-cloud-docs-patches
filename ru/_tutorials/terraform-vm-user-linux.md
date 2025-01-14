Чтобы добавить пользователя на создаваемую ВМ, в блоке `metadata` укажите параметр `user-data` с пользовательскими метаданными. Для этого:

1. Создайте текстовый файл с метаданными в кодировке UTF-8, например:

    {% include [user-data](../_includes/compute/user-data.md) %}

1. В файле `main.tf` вместо `ssh-keys` задайте параметр `user-data` и укажите путь к файлу с метаданными:

    ```
    metadata = {
        user-data = "${file("<путь к файлу>/meta.txt")}"
    }
    ```

Подробнее о работе с метаданными читайте в разделе [Метаданные виртуальной машины](../compute/concepts/vm-metadata).
