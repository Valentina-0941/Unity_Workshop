powered by Denisov D.V.

BigDigital лаборатория разработки игр 

[bigdigital-gamelab.ru](https://bigdigital-gamelab.ru)


### Работа #1 - Компонент физики Rigidbody в Unity

**Цель работы**: изучить свойства компонента Rigidbody, освоить реализацию игровых механик с применением свойств этого компонента


### Задание 1

Была создана сцена, на которой проводились различные эксперименты с физикой объектов в Unity. 

![png_1](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/png_1.png)

После добавления компонента **Rigidbody** на объекты CubeSid и CubeNancy, кубы начали падать вниз под действием гравитации.

![gif_1](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_1.gif)

Для эксперимента с массой были установлены разные значения: для CubeSid — 1 кг, для CubeNancy — 100 кг.

| ![png_2](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/png_2.png)|
|---|
| ![png_3](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/png_3.png) |

![gif_2](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_2.gif)

При запуске сцены оба куба падали с одинаковой скоростью, так как гравитация действует на объекты одинаково, независимо от их массы (при отсутствии сопротивления воздуха). Это подтверждает, что масса влияет на взаимодействие при столкновении, но не на скорость падения.

Затем было изменено значение свойства **Drag**: для CubeSid оно оставалось на 0, а для CubeNancy установили 10. В результате CubeSid достигал земли быстрее, что показало, как сопротивление влияет на скорость падения.

![gif_3](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_3.gif)

Далее сцена была дополнена сферой и эстокадой. Масса сферы была установлена 100 кг. При её наезде на CubeSid (1 кг) куб сдвинулся легко, а при наезде на CubeNancy (100 кг) сфера почти не повлияла на куб, так как их массы были одинаковыми.

![gif_4](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_4.gif)

![gif_5](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_5.gif) 

Затем были изменены параметры **Drag** и **Angular Drag** у CubeSid. При значениях **Drag = 0** и **Angular Drag = 0.05** объект продолжал вращаться, а при значениях **Drag = 2** и **Angular Drag = 2** вращение замедлялось быстрее. Это показало, что **Angular Drag** влияет на вращение объектов, и при значении, равном 0, объект продолжал вращаться без торможения. Такое поведение позволяет объектам сохранять естественную инерцию.

![gif_6](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_6.gif)

![gif_7](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_7.gif)

![gif_8](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_8.gif)

Далее на платформу **Stage** был добавлен компонент **Rigidbody** с отключённой гравитацией. При запуске сцены Stage оставался на месте до момента столкновения с другими объектами.

![gif_9](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_9.gif)

В настройках физики было увеличено значение силы гравитации. При значении до -140 кубы начали двигаться с рывками, а при гравитации выше -140 они начинали "проскакивать" сквозь текстуру. Это связано с тем, что движок не успевает обработать столкновения при слишком высокой скорости движения объектов. В дальнейшем значение гравитации было возвращено к настройкам по умолчанию.

> Работаю на ноутбуке со встроенной видеокартой(

![gif_10](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_10.gif)

Значение гравитации было возвращено к настройкам по умолчанию. 

Для объекта Stage были зафиксированы все три оси, чтобы платформа вращалась, но не сдвигалась в пространстве. Это показало, что при неправильной фиксации осей объект может сдвигаться.

![gif_11](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_11.gif)

Также было протестировано свойство **isKinematic**, которое делает объект нефизическим. При активации этого свойства на вращающемся объекте платформа остановилась, и при повторной деактивации вращение не возобновилось.

![gif_12](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_12.gif)

Наконец, объект CubeSid был поднят на высоту 1000 метров. При падении куб проскочил сквозь землю из-за высокой скорости и недостаточной частоты обновления физики. Решением этой проблемы может стать уменьшение шага симуляции или использование непрерывной детекции коллизий для точной обработки столкновений при высокой скорости движения.

![gif_13](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_13.gif)

Используя ниже приведенный скрипт, во время запуска сцены были изменены характеристики CubeSid такие как:
- Включение / отключение isKinematic  
- Добавление массы
- Изменение уровня гравитации, массы и угла сопративления

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

  
public class RigidbodyMod : MonoBehaviour
{
    private Rigidbody _rb;

    void Start()
    {
        _rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        // Увеличение массы на клавишу "M"
        if (Input.GetKeyDown(KeyCode.M))
        {
            _rb.mass += 1.0f;
            Debug.Log("Масса увеличена: " + _rb.mass);
        }

        // Сброс массы до 1 кг на клавишу "R"
        if (Input.GetKeyDown(KeyCode.R))
        {
            _rb.mass = 1.0f;
            Debug.Log("Масса сброшена: " + _rb.mass);
        }

        // Включение гравитации на клавишу "G"
        if (Input.GetKeyDown(KeyCode.G))
        {
            _rb.useGravity = true;
            Debug.Log("Гравитация включена");
        }

        // Отключение гравитации на клавишу "H"
        if (Input.GetKeyDown(KeyCode.H))
        {
            _rb.useGravity = false;
            Debug.Log("Гравитация отключена");
        }

        // Изменение сопротивления на клавишу "D"
        if (Input.GetKeyDown(KeyCode.D))
        {
            _rb.drag = 2.0f;
            Debug.Log("Сопротивление увеличено: " + _rb.drag);
        }

        // Изменение углового сопротивления на клавишу "A"
        if (Input.GetKeyDown(KeyCode.A))
        {
            _rb.angularDrag = 1.0f;
            Debug.Log("Угловое сопротивление увеличено: " + _rb.angularDrag);
        }

        // Активация isKinematic на клавишу "K"
        if (Input.GetKeyDown(KeyCode.K))
        {
            _rb.isKinematic = true;
            Debug.Log("isKinematic включено");
        }

        // Деактивация isKinematic на клавишу "L"
        if (Input.GetKeyDown(KeyCode.L))
        {
            _rb.isKinematic = false;
            Debug.Log("isKinematic отключено");
        }

        // Восстановление значений физики на Space
        if (Input.GetKeyDown(KeyCode.Space))
        {
            _rb.mass = 1.0f;
            _rb.drag = 0;
            _rb.angularDrag = 0.05f;
            _rb.useGravity = true;
            _rb.isKinematic = false;
            Debug.Log("Параметры физики сброшены к значениям по умолчанию");
        }
    }
}
```

![gif_14](https://github.com/Valentina-0941/Unity_Workshop/blob/main/source/W1_source/gif_14.gif)

Знания свойств Rigidbody помогают создавать реалистичное поведение объектов в играх и симуляторах. Они позволяют управлять взаимодействиями через физику — гравитацию, массу, сопротивление и вращение. Это важно для создания правдоподобных столкновений и движений объектов. Свойства вроде isKinematic дают возможность комбинировать физические взаимодействия с программируемыми действиями, что делает сцены интерактивными. Такие навыки важны для создания головоломок, игр с физическими задачами и реалистичных симуляторов.

