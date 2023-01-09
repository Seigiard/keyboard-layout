# Универсальная раскладка клавиатуры для US-клавиатур

Универсальная раскладка — пакет из английской и русско-белорусской-украинской раскладок для Мака, спроектированных для удобного совместного использования.

> US-клавиатура — это клавиатура, где горизонтально вытянутая клавиша Enter. INTL-клавиатура — это где клавиша Enter занимает два ряда.
>
> Как раскладка будет работать на Intl-клавиатуре — я пока не знаю.

## Проблемы

У меня накопилось много претензий к стандартной и нестандартной раскладкам:
- в русской раскладке не достучаться до `#`;
- в русской раскладке сложно набирать `<>[]{}`;
- каждый раз надо вспоминать, где в какой раскладке лежат `|\/`;
- тяжело переключаться между русской, белорусской и украинской раскладкой. На украинской сломано всё, что можно сломать;
- Прыгающие по кнопкам знаки препинания;
- в раскладке Бирмана на US-клавиатуре тильда под ESC прячется на третий слой и это ужасно.

## Решение

Можно выучить alt-коды всех нужных символов. Можно выучить option-слой в маковской раскладке (а он огромный, посмотрите). Можно всё, но...

**Можно сделать самому и ХУЖЕ**

Вдохновившись типографской раскладкой Бирмана и универсальной раскладкой Тонского, я решил сварганить своё поделие.

![Keyboard Layout, with transparent backgrount](./Seigiard-keyboard-layout.png)

При этом я старался придерживаться следующих правил:

### Правила

1. Основые слои (базовый и `shift`) остаются максимально as is
2. Все модификации вынесены на слои `opt` и `opt+shift` и работают на обеих раскладках.
3.

### Изменения в основном слое

1. Бэктик `` ` `` вызывается через `` opt+` `` в обеих раскладках
2. Тильда `` ~ `` вызывается через `` opt+shift+` `` в обеих раскладках
3. Слэши `\|`теперь стандартны в обеих раскладках
4. `#` теперь в обеих раскладках вызывается через `shift+3`

### Дополнительне слои `opt` и `opt+shift`

Смотрите картинку ниже, там показаны все дополнительные символы, доступные в раскладке

### Кириллическая раскладка

1. `№` — через `opt+3`
2. `[їўґі]` через `opt+(shift)+[йуги]` — удобно для мнемонического запоминания

![Keyboard Layout with descriptions](./Seigiard-keyboard-layout-with-description.png)

## Проблемы

Куда без проблем. При переназначении клавиши `` ` `` отвалилось переключение между окнами одного приложения (`` win+` ``)

Для решения на биндинг переключения раскладок в karabiner были повешены два дополнительных скрипта:

```clojure
;; Templates
:templates {
  :en-tilda-mapping "hidutil property --set '{\"UserKeyMapping\": []}'"
  :ru-tilda-mapping "hidutil property --set '{\"UserKeyMapping\": [{\"HIDKeyboardModifierMappingSrc\": 0x700000064, \"HIDKeyboardModifierMappingDst\": 0x700000035}, {\"HIDKeyboardModifierMappingSrc\": 0x700000035, \"HIDKeyboardModifierMappingDst\": 0x700000064}]}'"
}

 ;; Input Sources
 :input-sources {
  :en-seigiard {:input_source_id "^org.sil.ukelele.keyboardlayout.seigiardlayout.english-seigiardtypography$"}
  :ru-seigiard {:input_source_id "^org.sil.ukelele.keyboardlayout.seigiardlayout.russian-seigiardtypography$"}
}

 ;; BINDINGS
 :main [
  {
    :des "Left Cmd → Seigiard En, Right Cmd → Seigiard Ru"
    :rules [
      [:condi :!rdp :!parallels]
      [:left_command      :left_command  nil  {:alone [{:input :en-seigiard} [:en-tilda-mapping]]}]
      [:right_command     :right_command nil {:alone [{:input :ru-seigiard} [:ru-tilda-mapping]]}]
    ]
  }
]
```

# Universal Keyboard Layout

Another approach to solve common problem.

Inspired by [Tonsky's Universal Layout](https://github.com/tonsky/Universal-Layout). Based on [Ilya Birman′s typographic layout](https://ilyabirman.ru/typography-layout/).

