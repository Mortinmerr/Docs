# Синтаксис документирования базовых сущностей
В данном разделе представлены шаблоны и примеры оформления основных частей кода

## Шапка файла:
  Любой документ должен начинаться с шапки оформленной блоком многострочного комментария. 

#### Шаблон блока документации:

  ```C
    /******************************************************************************
    * @file    file.h или file.c
    * @author  Ваше имя, или корпоративная почта, или никнейм в Git
    * @date    дата создания документа
    * @version текущая версия документа (если она ведется для этого исходника)
    *
    * @brief   Короткое оглавление, описывающее назначение. 1-2 строки.
    *
    * @details Многострочное описание содержимого.
    *          Под этим тегом вы можете дать более подробное описание, привести
    *          кратко теорию, или абстракции, которые нужно знать, для понимания
    *          кода и интерфейса. 
    *
    * @todo    Заметка на будущее. Функционал или дополнение, которое нужно сделать,
               но сейчас нет возможности. Например, API реализован, 
               но не реализован бекэнд для части API.
    * @todo    Их может быть несколько. 
    ******************************************************************************/
  ```

#### Пример использования:
  ```C
    /******************************************************************************
     * @file    SMHAL_AXIGP.h
     * @author  Valitov_TE
     * @date    2025-08-28
     * @version 1.1.1
     *
     * @brief   SMHAL_AXIGP - Self Made Hardware Abstraction Layer AXI_GP
     *
     * @details This library is a wrapper for accessing the low-level AXI GP bus.
     * It was created to decouple hardware details, eliminate dependencies on
     * platform libraries, and isolate the rest of the code from direct hardware
     * access.
     *
     * Important note:
     * Why was this code moved into a separate AXI library?
     * In the case of our THWP (Target Hardware Platform), AXI_GP cannot fully
     * utilize the AXI functionality. The purpose of this core is simple
     * register access to peripheral devices with minimal logic in PL.
     * As a result, we cannot use burst operations. Instead, we perform only
     * single transactions (both reads and writes).
     *
     * @todo Implement error detection and handling when platform allows it.
     ******************************************************************************/
  ```

## Структуры, перечисления, типы:
  * Документирование этих типов данных обязательно при каждом объявлении.
  * Для документирования используйте блок многострочного комментария. 

#### Шаблон блока документации:
  ```C
  /**
   * @brief     Краткое, однострочное описание.
   *
   * @details   Развернутое описание. Не обязательное поле.
   *
   * @code - можно использовать для примера кода, обращающегося к
   *            описанному типу. Не обязательное поле.
   * @endcode
   */
  typedef struct {
    тип_переменной имя_переменной; /**< Документирование члена многострочным комментарием. 
                                          Спец символ "<", следом за "/**" */
    тип_переменной имя_переменной; ///< Так же работает и с однострочным комментарием
  } NameStruct_t;
  ```
  Примечание: 
  * Использовать можно оба варианта в зависимости от удобства,но предпочтительнее многострочным коммементарием.  
  * Для описания составной структуры данных можно использовать сокращенный комментарий.

#### Пример использования:
```C
/**
 * @brief Container for current library version string.
 *
 * @details Created and filled automatically in AXIDev_DescCreate().
 *
 * @code
 * xil_printf("AXIDev Version: %s\n", AXIDevices_h->CurrVerOfLib->VersionStr_p);
 * @endcode
 */
typedef struct {
  const char*    VersionStr_p; /**< Version string */
  const uint16_t StrLength;    /**< String length in bytes */
} AXIDevs_Ver_t;

/** Return Codes */
typedef enum {
  AXIDevs_Succ         = 0x01u,
  AXIDevs_Fail         = 0x02u,
  AXIDevs_WriteSucc    = 0x03u,
  AXIDevs_WriteFail    = 0x04u,
} AXIDevs_ReCo_t;
```  

## Макросы

### Макрос-константа
  * Фиксированное число или флаг, "#if" или значение по умолчанию.
  * Для документирования используйте блок многострочного комментария.
  
  
#### Шаблон блока документации:
  ```C
  /** @brief краткое однострочное описание*/
  ```

#### Пример использования:
  ```C
  /** @brief Default timeout, ms. */
  #ifndef MOD_TIMEOUT_MS
  #define MOD_TIMEOUT_MS 100u
  #endif

  /** @brief Error flag bit. */
  #define MOD_STATUS_ERR (1u << 0)
  ```

### Макрос-функция
  * Простое вычисление "в одну строку" или макро-подстановка.
  * Для документирования используйте блок многострочного комментария.

#### Шаблон блока документации:
```C
  /** @brief     Краткое, однострочное описание.
   *  @param[in] Тег для описания параметра функции, его направление,
   * ...              и расшифровка-описание назначения или содержания.
   *  @param[in] Тег @param с наполнением должен быть для каждого параметра
   *  @return     Описание возвращаемого значения. Указать тип, 
   *                  и возвращаемое значение, в случае успешного выполнения.
   */
```
#### Пример использования:
```C
  /** @brief     Smaller of a,b.
   *  @param[in] a  Comp. variable
   *  @param[in] b  Comp. variable
   *  @return    a if a<b, else b.
   */
  #define MIN(a,b) (((a) < (b)) ? (a) : (b))
  
  /** @brief Single-bit mask.
   *  @param[in] n Bit index [0..31].
   */
  #define BIT(n) (1u << (n))
```

### Функции:
  * Для документирования функций используйте блок многострочного 
    комментария.
#### Шаблон блока документации:
```C
  /**
    * @brief           Краткое, однострочное описание.
    *
    * @details         Развернутое описание. Не обязательное поле.
    *
    * @param[in,out]   Тег для описания параметра функции, его направление,
    * ...              и расшифровка-описание назначения или содержания.
    * @param[in,out]   Тег @param с наполнением должен быть для каждого параметра
    *
    * @return          Описание возвращаемого значения. Указать тип, 
    *                  и возвращаемое значение, в случае успешного выполнения.
    *
    *
    * @par Example     Комбинация "@par Example" и последующий тег "@code"
    *                  позволяет разместить в документации код-пример
    *                  использования функции. Не обязательное поле.
    * @code{.c}
    *   код-пример
    * @endcode
    */
  тип_переменной имя_переменной (входные_параметры);
```
#### Пример использования:
```C
  /**
    * @brief    Function for summing two numbers
    *
    * @param[in]  a  The first term
    * @param[in]  b  The second term
    *
    * @return  The sum of two numbers
    *
    * @par Example
    * @code{.c}
    *   int sum_result = add(2, 3); // result = 5
    * @endcode
    */
  int add (int a, int b);
```
* Документация, в обязательном порядке, должна быть написана к функции, если:
  - это объявление или определение в \*.h файле.
  - это определение в \*.с файле.

* Дублирование документации к определению функции в \*.с файле,
  из объявления в \*.h файле, не обязательно, но приветствуется.
