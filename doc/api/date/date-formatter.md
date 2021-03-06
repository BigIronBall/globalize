## .dateFormatter( pattern ) ➜ function( value )

Return a function that formats a date according to the given `pattern`.

The returned function is invoked with one argument: the Date instance `value` to
be formatted.

### Parameters

**pattern**

String value indicating a skeleton, eg. `"GyMMMd"`.

> Skeleton provides a more flexible formatting mechanism than the predefined
> list `full`, `long`, `medium`, or `short` represented by date, time, or
> datetime.  Instead, they are an open-ended list of patterns containing
> only [date
> field](http://www.unicode.org/reports/tr35/tr35-dates.html#Date_Field_Symbol_Table)
> information, and in a canonical order. For example:
>
> | locale | `"GyMMMd"` skeleton |
> | --- | --- |
> | *en* | `"Apr 9, 2014 AD"` |
> | *zh* | `"公元2014年4月9日"` |
> | *es* | `"9 abr. de 2014 d. C."` |
> | *ar* | `"٩ أبريل، ٢٠١٤ م"` |
> | *pt* | `"9 de abr de 2014 d.C."` |

Or, a JSON object including one of the following.

> **skeleton**
>
> String value indicating a skeleton (see description above), eg.
> `{ skeleton: "GyMMMd" }`.
>
> **date**
> One of the following String values: `full`, `long`, `medium`, or `short`, eg.
> `{ date: "full" }`.
>
> **time**
>
> One of the following String values: `full`, `long`, `medium`, or `short`, eg.
> `{ time: "full" }`.
>
> **datetime**
>
> One of the following String values: `full`, `long`, `medium`, or `short`, eg.
> `{ datetime: "full" }`
>
> **pattern**
>
> String value indicating a
> [raw pattern](http://www.unicode.org/reports/tr35/tr35-dates.html#Date_Field_Symbol_Table)
> eg. `{ pattern: "dd/mm" }`.
>
> Raw patterns are NOT recommended for i18n in general. Few specific cases may
> suite the need of using it, eg. locale-independent format for machine parsing.
>
> Use skeletons for i18n purposes.

**value**

Date instance to be formatted, eg. `new Date()`;

### Example

Prior to using any date methods, you must load
`cldr/main/{locale}/ca-gregorian.json`, `cldr/main/{locale}/timeZoneNames.json`,
`cldr/supplemental/timeData.json`, `cldr/supplemental/weekData.json`, and the
CLDR content required by the number module. Read [CLDR content][] if you need
more information.

[CLDR content]: ../../../README.md#2-cldr-content

You can use the static method `Globalize.dateFormatter()`, which uses the default
locale.

```javascript
var formatter;

Globalize.locale( "en" );
formatter = Globalize.dateFormatter({ datetime: "short" });

formatter( new Date( 2010, 10, 30, 17, 55 ) );
// > "11/30/10, 5:55 PM"
```

You can use the instance method `.dateFormatter()`, which uses the instance locale.

```javascript
var enFormatter = Globalize( "en" ).dateFormatter({ datetime: "short" }),
  deFormatter = Globalize( "de" ).dateFormatter({ datetime: "short" });

enFormatter( new Date( 2010, 10, 30, 17, 55 ) );
// > "11/30/10, 5:55 PM"

deFormatter( new Date( 2010, 10, 30, 17, 55 ) );
// > "30.11.10 17:55"
```

For comparison, follow the same formatter using different locales.

| locale | `Globalize( locale ).dateFormatter({ datetime: "short" })( new Date( 2010, 10, 1, 17, 55 ) )` |
| --- | --- |
| *en* | `"11/1/10, 5:55 PM"` |
| *en_GB* | `"01/11/2010 17:55"` |
| *zh* | `"10/11/1 下午5:55"` |
| *zh-u-nu-native* | `"一〇/一一/一 下午五:五五"` |
| *es* | `"1/11/10 17:55"` |
| *de* | `"01.11.10 17:55"` |
| *pt* | `"01/11/10 17:55"` |
| *ar* | `"١‏/١١‏/٢٠١٠ ٥،٥٥ م"` |

Use convenient presets for `date`, `time`, or `datetime`. Their possible values
are: `full`, `long`, `medium`, or `short`.

| `presetValue` | `Globalize( "en" ).dateFormatter( presetValue )( new Date( 2010, 10, 1, 17, 55 ) )` |
| --- | --- |
| `{ date: "short" }` | `"11/1/10"` |
| `{ date: "medium" }` | `"Nov 1, 2010"` |
| `{ date: "long" }` | `"November 1, 2010"` |
| `{ date: "full" }` | `"Monday, November 1, 2010"` |
| `{ time: "short" }` | `"5:55 PM"` |
| `{ time: "medium" }` | `"5:55:00 PM"` |
| `{ time: "long" }` | `"5:55:00 PM GMT-2"` |
| `{ time: "full" }` | `"5:55:00 PM GMT-02:00"` |
| `{ datetime: "short" }` | `"11/1/10, 5:55 PM"` |
| `{ datetime: "medium" }` | `"Nov 1, 2010, 5:55:00 PM"` |
| `{ datetime: "long" }` | `"November 1, 2010 at 5:55:00 PM GMT-2"` |
| `{ datetime: "full" }` | `"Monday, November 1, 2010 at 5:55:00 PM GMT-02:00"` |

Use skeletons for more flexibility (see its description [above](#parameters)).

| `skeleton` | `Globalize( "en" ).dateFormatter( skeleton )( new Date( 2010, 10, 1, 17, 55 ) )` |
| --- | --- |
| `"E"` | `"Tue"` |
| `"EHm"` | `"Tue 17:55"` |
| `"EHms"` | `"Tue 17:55:00"` |
| `"Ed"` | `"30 Tue"` |
| `"Ehm"` | `"Tue 5:55 PM"` |
| `"Ehms"` | `"Tue 5:55:00 PM"` |
| `"Gy"` | `"2010 AD"` |
| `"GyMMM"` | `"Nov 2010 AD"` |
| `"GyMMMEd"` | `"Tue, Nov 30, 2010 AD"` |
| `"GyMMMd"` | `"Nov 30, 2010 AD"` |
| `"H"` | `"17"` |
| `"Hm"` | `"17:55"` |
| `"Hms"` | `"17:55:00"` |
| `"M"` | `"11"` |
| `"MEd"` | `"Tue, 11/30"` |
| `"MMM"` | `"Nov"` |
| `"MMMEd"` | `"Tue, Nov 30"` |
| `"MMMd"` | `"Nov 30"` |
| `"Md"` | `"11/30"` |
| `"d"` | `"30"` |
| `"h"` | `"5 PM"` |
| `"hm"` | `"5:55 PM"` |
| `"hms"` | `"5:55:00 PM"` |
| `"ms"` | `"55:00"` |
| `"y"` | `"2010"` |
| `"yM"` | `"11/2010"` |
| `"yMEd"` | `"Tue, 11/30/2010"` |
| `"yMMM"` | `"Nov 2010"` |
| `"yMMMEd"` | `"Tue, Nov 30, 2010"` |
| `"yMMMd"` | `"Nov 30, 2010"` |
| `"yMd"` | `"11/30/2010"` |
| `"yQQQ"` | `"Q4 2010"` |
| `"yQQQQ"` | `"4th quarter 2010"` |

```javascript
var globalize = Globalize( "en" ),
  date = new Date( 2010, 10, 30, 17, 55 ),
  monthDayFormatter = globalize.dateFormatter( "MMMd" ),
  hourMinuteSecondFormatter = globalize.dateFormatter( "Hms" );

monthDayFormatter( date );
// > "Nov 30"

hourMinuteSecondFormatter( date );
// > "17:55:00"
```

For improved performance on iterations, first create the formatter. Then, reuse
it on each loop.

```javascript
var dates = [ new Date( a ), new Date( b ), ... ];
var formatter = Globalize( "en" ).dateFormatter({ time: "short" });

formattedDates = dates.map(function( date ) {
  return formatter( date );
});
```
