# Griefly Visual Studio 2022 Solution

Эта `.sln` включает все ключевые подпроекты как NMake-проекты, которые вызывают CMake-сборку конкретных таргетов.
### Что это даёт
- **Solution Explorer** покажет все проекты: KVEngine, KVClient, Launcher, Unpacker, MapEditor, BasicAssetsGenerator, GoServer.
- Сборка любого проекта в решении фактически вызывает `cmake --build ... --target <имя>`, т.е. используется оригинальная CMake-логика (Qt/moc/uic, генерация файлов и т.п.).

## Подготовка
1. Открой **x64 Native Tools Command Prompt for VS 2022** **или** выбери этот профиль терминала в VS Code.
2. В корне проекта выполните:
   ```cmd
   cmake --preset windows-msvc
   ```
   Это создаст `build/windows-msvc/` и сгенерирует CMake-проекты для MSVC.

## Сборка из Visual Studio 2022
1. Открой `Griefly.sln`.
2. Выбери конфигурацию: `Debug` или `Release`, платформа `x64`.
3. Щёлкни правой кнопкой по нужному проекту → **Build**.
   - `KVClient` (клиент)
   - `KVEngine` (движок, DLL)
   - `Launcher` (Qt GUI лаунчер)
   - `Unpacker`, `MapEditor`, `BasicAssetsGenerator` (утилиты)
   - `GoServer` (сборка Go)
4. Для Go-сервера должен быть установлен **Go**. Сборка идёт командой `go build` в `gopath/src/griefly-server`.

## Сборка из VS Code (терминал)
```cmd
cmake --preset windows-msvc
cmake --build build\windows-msvc --config Debug --target KVClient
```
