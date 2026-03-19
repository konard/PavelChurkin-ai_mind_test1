# Варианты настройки OpenClaw

[OpenClaw](https://github.com/pjasicek/OpenClaw) — это open-source переосмысление платформера *Captain Claw* (1997) на C++/SDL2. Все настройки управляются через файл `config.xml` и встроенную консоль разработчика.

---

## Расположение файла конфигурации

Движок ищет `config.xml` в следующем порядке:

| Платформа | Путь |
|---|---|
| Linux | `~/.config/openclaw/config.xml` |
| Windows | та же папка, что и исполняемый файл |
| Android | `/sdcard/claw/config.xml` |

Если `config.xml` не найден, при первом запуске он создаётся автоматически со значениями по умолчанию.

---

## Структура config.xml

Файл имеет корневой элемент `<Configuration>` со следующими секциями:

---

### `<Display>` — Настройки дисплея

| XML-элемент | По умолчанию | Описание |
|---|---|---|
| `<Size width="..." height="..."/>` | 1280 × 960 | Разрешение окна/рендеринга в пикселях |
| `<Scale>` | 1 | Множитель масштаба рендеринга (например, 2 — масштаб 2x) |
| `<UseVerticalSync>` | false | Включить/выключить вертикальную синхронизацию (VSync). Предотвращает разрывы кадров, может добавить задержку ввода |
| `<IsFullscreen>` | false | Полноэкранный режим |
| `<IsFullscreenDesktop>` | false | Безрамочный оконный полноэкранный режим («рабочий стол в полный экран») |

**Пример:**
```xml
<Display>
    <Size width="1920" height="1080"/>
    <Scale>1</Scale>
    <UseVerticalSync>true</UseVerticalSync>
    <IsFullscreen>false</IsFullscreen>
    <IsFullscreenDesktop>true</IsFullscreenDesktop>
</Display>
```

---

### `<Audio>` — Настройки звука

| XML-элемент | По умолчанию | Описание |
|---|---|---|
| `<Frequency>` | 44100 | Частота дискретизации звука в Гц |
| `<SoundChannels>` | 2 | Количество каналов вывода (1 = моно, 2 = стерео) |
| `<MixingChannels>` | 64 | Количество одновременно воспроизводимых звуков (SDL_mixer каналы) |
| `<ChunkSize>` | 2048 | Размер звукового буфера в сэмплах. Больше = меньше треска, но больше задержка |
| `<SoundVolume>` | 10 | Громкость звуковых эффектов (0–100) |
| `<MusicVolume>` | 1 | Громкость музыки (0–100) |
| `<SoundOn>` | true | Включить/выключить все звуковые эффекты |
| `<MusicOn>` | true | Включить/выключить музыку |
| `<MusicRpcServerPath>` | MidiProc.exe | Путь к вспомогательному процессу MIDI (только Windows) |

**Пример:**
```xml
<Audio>
    <Frequency>44100</Frequency>
    <SoundChannels>2</SoundChannels>
    <MixingChannels>64</MixingChannels>
    <ChunkSize>2048</ChunkSize>
    <SoundVolume>50</SoundVolume>
    <MusicVolume>30</MusicVolume>
    <SoundOn>true</SoundOn>
    <MusicOn>true</MusicOn>
</Audio>
```

> **Примечание для Linux:** для воспроизведения MIDI-музыки необходимо установить `timidity` (или `timidity++`) и `freepats`.

---

### `<Assets>` — Ресурсы и файлы

| XML-элемент | По умолчанию | Описание |
|---|---|---|
| `<AssetsFolder>` | *(пусто)* | Дополнительный путь к папке с ресурсами |
| `<RezArchive>` | CLAW.REZ | Имя архива с ресурсами оригинальной игры (обязательно) |
| `<CustomArchive>` | ASSETS.ZIP | Архив с дополнительными ресурсами OpenClaw |
| `<ResourceCacheSize>` | 150 | Максимальный объём памяти (МБ) для кэша ресурсов |
| `<TempDir>` | *(пусто)* | Временная директория |
| `<SavesFile>` | SAVES.XML | Имя файла сохранений |

**Пример:**
```xml
<Assets>
    <RezArchive>CLAW.REZ</RezArchive>
    <CustomArchive>ASSETS.ZIP</CustomArchive>
    <ResourceCacheSize>200</ResourceCacheSize>
    <SavesFile>SAVES.XML</SavesFile>
</Assets>
```

---

### `<GlobalOptions>` — Игровой баланс и физика

| XML-элемент | По умолчанию | Описание |
|---|---|---|
| `<MaxJumpSpeed>` | 8.8 | Максимальная скорость прыжка вверх |
| `<MaxFallSpeed>` | 14.0 | Максимальная скорость падения |
| `<ClawRunningSpeed>` | 4.5 | Горизонтальная скорость бега Клоу |
| `<MaxJumpHeight>` | 155 | Максимальная высота прыжка в пикселях |
| `<PowerupMaxJumpHeight>` | 205 | Высота прыжка при активном усилении |
| `<SpringBoardSpringSpeed>` | 11 | Скорость запуска с батута |
| `<ClawMinFallHeight>` | 500 | Минимальная высота падения (пикс.) до урона от падения |
| `<IdleSoundQuoteInterval>` | 15000 | Интервал (мс) между звуковыми цитатами в режиме ожидания |
| `<PlatformSpeedModifier>` | 0.015 | Множитель скорости движущихся платформ |
| `<StartLookUpOrDownTime>` | 1500 | Задержка (мс) перед прокруткой камеры вверх/вниз |
| `<FreezeTime>` | 2000 | Длительность (мс) эффекта заморозки |
| `<MaxLookUpOrDownDistance>` | 250 | Максимальное смещение камеры при взгляде вверх/вниз (пикс.) |
| `<LookUpOrDownSpeed>` | 250 | Скорость прокрутки камеры (пикс./сек) |
| `<ShowFps>` | false | Показывать счётчик кадров в секунду (FPS) |
| `<ShowPosition>` | false | Показывать текущую позицию Клоу |
| `<LoadAllLevelSaves>` | false | Загружать сохранения для всех уровней при старте |

**Пример (изменение скорости и отображения FPS):**
```xml
<GlobalOptions>
    <ClawRunningSpeed>5.0</ClawRunningSpeed>
    <MaxJumpSpeed>9.0</MaxJumpSpeed>
    <ShowFps>true</ShowFps>
</GlobalOptions>
```

---

### `<ControlOptions>` — Управление

| XML-элемент | По умолчанию | Описание |
|---|---|---|
| `<UseAlternateControls>` | false | Использовать альтернативную раскладку управления |
| `<TouchScreen><Enable>` | true | Включить обработку сенсорного ввода |
| `<TouchScreen><DistanceThreshold>` | 0.05 | Порог смещения касания (доля экрана) для разделения тапов и свайпов |
| `<TouchScreen><TimeThreshold>` | 100 | Временной порог (мс) для разделения коротких и длинных касаний |

> **Контроллеры:** для настройки геймпада можно задать переменную окружения `SDL_GAMECONTROLLERCONFIG`.

---

### `<DebugOptions>` — Отладка

| XML-элемент | По умолчанию | Описание |
|---|---|---|
| `<SkipMenu>` | false | Пропустить главное меню при запуске (перейти сразу к уровню) |
| `<SkipMenuToLevel>` | 13 | Номер уровня для перехода при `SkipMenu=true` |
| `<LastImplementedLevel>` | 13 | Максимально доступный уровень в меню |
| `<SkipBossFightIntro>` | false | Пропускать заставки перед боссами |
| `<CpuDelay>` | 0 | Искусственная задержка (мс) на каждый кадр (для тестирования) |

**Пример (быстрый запуск для разработки):**
```xml
<DebugOptions>
    <SkipMenu>true</SkipMenu>
    <SkipMenuToLevel>1</SkipMenuToLevel>
    <SkipBossFightIntro>true</SkipBossFightIntro>
</DebugOptions>
```

---

### `<Console>` — Встроенная консоль разработчика

Консоль открывается клавишей `~` (тильда).

| XML-элемент | По умолчанию | Описание |
|---|---|---|
| `<BackgroundImagePath>` | console02.tga | Путь к фоновому изображению консоли |
| `<StretchBackgroundImage>` | true | Растягивать фон по размеру консоли |
| `<WidthRatio>` | 1.0 | Ширина консоли как доля от ширины окна (0.0–1.0) |
| `<HeightRatio>` | 0.5 | Высота консоли как доля от высоты окна (0.0–1.0) |
| `<ConsoleAnimationSpeed>` | 0.7 | Скорость анимации выезда консоли |
| `<FontPath>` | clacon.ttf | Файл шрифта для текста консоли |
| `<FontColor r="..." g="..." b="..."/>` | 255,255,255 | Цвет текста консоли (RGB) |
| `<FontHeight>` | 20 | Размер шрифта в консоли |
| `<LeftOffset>` | 5 | Горизонтальный отступ от левого края (пикс.) |
| `<CommandPrompt>` | `> ` | Строка приглашения перед вводом команды |

---

## Команды встроенной консоли

Консоль открывается клавишей `~`. Команды нечувствительны к регистру.

### Чит-коды и игровые команды

| Команда | Описание |
|---|---|
| `InfiniteAmmo on/off` | Бесконечные боеприпасы |
| `Invincible on/off` | Неуязвимость |
| `InfiniteJump on/off` | Бесконечные прыжки |
| `PhysicsDebug on/off` | Отображение физических коллайдеров |
| `catnip` | Применить усиление Catnip на 30 секунд |
| `firesword` | Применить усиление Fire Sword на 30 секунд |
| `frostsword` | Применить усиление Frost Sword на 30 секунд |
| `lightningsword` | Применить усиление Lightning Sword на 30 секунд |
| `invisible` / `invisibility` / `invis` | Невидимость на 30 секунд |
| `invulnerable` / `invlr` | Неуязвимость на 30 секунд |
| `spawn coin` | Заспавнить монету рядом с Клоу |
| `spawn skull` | Заспавнить череп рядом с Клоу |
| `teleport <x> <y>` | Телепортировать Клоу в координаты x, y |

### Системные и отладочные команды

| Команда | Описание |
|---|---|
| `reset level` / `rl` | Перезапустить текущий уровень |
| `menu` | Вернуться в главное меню |
| `cpudelay <мс>` | Установить искусственную задержку кадра в мс |
| `winresize <ширина> <высота> <масштаб>` | Изменить размер окна |
| `reload levelmetadata` | Горячая перезагрузка метаданных уровня |
| `reload actorprototypes` / `reload actorproto` | Горячая перезагрузка прототипов актёров |
| `reload all` / `ra` | Полная горячая перезагрузка всех данных |

> **Подсказка:** строки, начинающиеся с `#`, воспринимаются как комментарии и игнорируются.

---

## Файл стартовых команд (startup_commands.txt)

Файл `startup_commands.txt` в папке с исполняемым файлом читается при каждом запуске. Каждая строка выполняется как команда консоли. Это позволяет автоматизировать настройки без изменения `config.xml`.

**Пример `startup_commands.txt`:**
```
# Включить показ FPS
# (используйте консольные команды, применимые при старте)
InfiniteAmmo on
```

---

## Сводная таблица — что менять в первую очередь

| Задача | Что изменить | Где |
|---|---|---|
| Изменить разрешение | `<Size width="..." height="..."/>` | `<Display>` |
| Полноэкранный режим | `<IsFullscreen>true</IsFullscreen>` | `<Display>` |
| Убрать разрывы кадров | `<UseVerticalSync>true</UseVerticalSync>` | `<Display>` |
| Изменить громкость | `<SoundVolume>`, `<MusicVolume>` | `<Audio>` |
| Выключить музыку | `<MusicOn>false</MusicOn>` | `<Audio>` |
| Ускорить Клоу | `<ClawRunningSpeed>` | `<GlobalOptions>` |
| Показать FPS | `<ShowFps>true</ShowFps>` | `<GlobalOptions>` |
| Пропустить меню при тестировании | `<SkipMenu>true</SkipMenu>` | `<DebugOptions>` |
| Бесконечные прыжки (тест) | `InfiniteJump on` в консоли | Консоль (~) |
| Кастомный архив игры | `<RezArchive>` | `<Assets>` |

---

## Ссылки

- [Репозиторий OpenClaw на GitHub](https://github.com/pjasicek/OpenClaw)
- [Исходный config.xml по умолчанию](https://github.com/pjasicek/OpenClaw/blob/master/Build_Release/config.xml)
- [Исходный код обработки команд консоли](https://github.com/pjasicek/OpenClaw/blob/master/OpenClaw/Engine/GameApp/CommandHandler.cpp)
- [Исходный код структур конфигурации](https://github.com/pjasicek/OpenClaw/blob/master/OpenClaw/Engine/GameApp/BaseGameApp.h)
