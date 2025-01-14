# ใบงานที่ 8-4 สร้าง Task สำหรับการนับเลขและ scan display  

## 4 เขียน code สำหรับการแสดงผลไปไว้ใน FreeRTOS Task

รายละเอียดเกี่ยวกับการสร้าง Task บน ESP-IDF สามารถศึกษาได้จากที่นี่ [FreeRTOS (IDF)](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/freertos_idf.html)


4.1 สร้าง task สำหรับการแสดงผลบน LED 2 หลักสลับกัน

```cpp
#include <stdio.h>
#include "SevenSegment.h"
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

SevenSegment s1(0);
SevenSegment s2(4);

// create counter as global variable
uint8_t counter = 0;

// create task handle for LED display task
TaskHandle_t xSevenSegmentHandle = NULL;

// create task and move code into it
void vTaskScanSevenSegment(void *Parameters)
{
    while (1)
    {
        s1.DisplayNumber(counter / 10);
        s1.DisplayOn();
        vTaskDelay(10 / portTICK_PERIOD_MS);
        s1.DisplayOff();

        s2.DisplayNumber(counter % 10);
        s2.DisplayOn();
        vTaskDelay(10 / portTICK_PERIOD_MS);
        s2.DisplayOff();
    }
}


// in main, create display task
extern "C" void app_main(void)
{
    xTaskCreate(vTaskScanSevenSegment, 
      "Seven Seg", 1024, NULL, 
      tskIDLE_PRIORITY, &xSevenSegmentHandle);
}
```
4.2 ทดสอบ build และรันโปรแกรม จะต้องปรากฏเลข 00 บน seven segment

![457804715_487273790777187_6707682876716440831_n](https://github.com/user-attachments/assets/52a96fbb-4c41-4496-9b0c-8bbfe2d05753)


จากการทดลองข้างบน พบว่าจอกระพริบ เนื่องจากเราสร้าง task ที่มีความสำคัญเป็น idle หมายความว่าจะแสดงผลเมื่อ CPU ว่างเท่านั้น

เพื่อให้แสดงผลต่อเนื่อง เราต้องเพิ่มความสำคัญของ task ให้มากขึ้น 

4.2.1 ใน app_main() ให้เปลี่ยน tskIDLE_PRIORITY เป็นตัวเลข 10

![alt text](./Pictures/image-11.png)


4.2.2 ทดสอบ build และรันโปรแกรม 

![ภาพ](https://github.com/user-attachments/assets/567a6df7-9d36-4b36-b964-c0e631592b53)

4.2.3 เมื่อ work แล้ว ให้ถ่ายคลิป และส่งงานขึ้น github 
![457730446_1751059359001088_1852619157765234998_n](https://github.com/user-attachments/assets/9ddc5172-0481-4926-a9cf-58bb75b158c7)

วิดิโอ ไฟล์

[[https://vt.tiktok.com/ZS2uk4edc/](https://vt.tiktok.com/ZS2ukwrVA/)](https://vt.tiktok.com/ZS2ukwrVA/)

4.3 สร้าง task สำหรับนับตัวเลข

4.3.1 แก้ code ใน main.cpp ดังนี้

```cpp
#include <stdio.h>
#include "SevenSegment.h"
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"

SevenSegment s1(0);
SevenSegment s2(4);

// create counter as global variable
uint8_t counter = 0;


// create task handle for LED display task
TaskHandle_t xSevenSegmentHandle = NULL;

// create task handle for counter task
TaskHandle_t xCounterHandle = NULL;

void vTaskScanSevenSegment(void *Parameters)
{
    while (1)
    {
        s1.DisplayNumber(counter / 10);
        s1.DisplayOn();
        vTaskDelay(10 / portTICK_PERIOD_MS);
        s1.DisplayOff();

        s2.DisplayNumber(counter % 10);
        s2.DisplayOn();
        vTaskDelay(10 / portTICK_PERIOD_MS);
        s2.DisplayOff();
    }
}

// add counter task, increment every 1000ms (1 second)
void vTaskCounter(void *Parameters)
{
    while (1)
    {
        if(counter++ > 99)
            counter = 0;
        vTaskDelay(1000 / portTICK_PERIOD_MS);
    }
}

extern "C" void app_main(void)
{
    xTaskCreate(vTaskScanSevenSegment, "Seven Seg", 
        1024, NULL, 10, &xSevenSegmentHandle);

    // create counter task with priority 5 (Lower than scan display task)   
    xTaskCreate(vTaskCounter, "Counter", 
        1024, NULL, 5, &xCounterHandle);
}
```

4.3.2 ทดสอบ build และรันโปรแกรม 

4.3.4 เมื่อ work แล้ว ให้ถ่ายคลิป และส่งงานขึ้น github 

ไฟล์ วิดิโอ

[[https://vt.tiktok.com/ZS2uk4edc/](https://www.tiktok.com/@ananu0001/video/7414213130475162898)](https://www.tiktok.com/@ananu0001/video/7414213130475162898)



งาน https://github.com/AnchisaPhetnoi/8-1-Seven-Segment-2-Digits.git

งาน
https://github.com/AnchisaPhetnoi/Lab-07-Seven-segment-Component-develope.git
