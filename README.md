# Тестовое задание №1:

Есть список директорий неизвестно насколько большой вложенности. В директории может быть файл count. Нужно пройтись по всем директориям и вернуть сумму всех чисел из файла count (файлов count может быть много)

## Решение:

```php
/**
 * Get sum from files
 *
 * @param string $directory - path to working directory
 * @param string $searchFileName - file name contains integer value
 * @return int
 * @throws Exception
 */

function getSumFromFiles(string $directory, string $searchFileName) : int
{
    if (!is_dir($directory)) {
        throw new \Exception('Directory doesn\'t exist');
    }

    # Both classes include to standard PHP Library (SPL). Compatible with PHP 5, PHP 7, PHP 8
    $directoryIterator = new \RecursiveDirectoryIterator($directory);
    $iterator = new \RecursiveIteratorIterator($directoryIterator);

    $sum = 0;

    foreach ($iterator as $file) {
        if ($file->getFileName() !== $searchFileName) {
            continue;
        }
        if (!$file->isFile()) {
            continue;
        }
        if (!$file->isReadable()) {
            continue;
        }
        $sum += (int) file_get_contents($file->getPathName());
    }

    return $sum;
}

try {
    echo getSumFromFiles('files', 'count');
} catch (Exception $e) {
    echo $e->getMessage();
}

```