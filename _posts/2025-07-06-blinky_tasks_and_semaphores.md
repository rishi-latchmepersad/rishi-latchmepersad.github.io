---
title: Blinky Tasks and Semaphores
---

The ESP32 is turning out to be a ton of fun. This morning, I watched two videos from EspressIf's DevCon, explaining their IOT Development Framework (ESP IDF).
and how it's built on top of FreeRTOS. 

The videos are available <a href="https://www.youtube.com/watch?v=J8zc8mMNKtc">here </a> and <a href="https://www.youtube.com/watch?v=03Q8oilo_2U&pp=0gcJCcEJAYcqIYzv">here</a>.

Darian Leung moves pretty quickly in his videos, but his explanations are pretty clear and I was able to follow along pretty easily.

Armed with his knowledge, I came back to the simple LED blinky program. I realized that it was written as a simple 'Superloop':

```
void app_main(void)
{

    /* Configure the peripheral according to the LED type */
    configure_led();

    while (1) {
        ESP_LOGI(TAG, "Turning the LED %s!", s_led_state == true ? "ON" : "OFF");
        blink_led();
        /* Toggle the LED state */
        s_led_state = !s_led_state;
        vTaskDelay(CONFIG_BLINK_PERIOD / portTICK_PERIOD_MS);
    }
}
```

This app main function turned out to be the entry point for all of the IDF. As a first step to experimenting with it, I therefore moved all the code to a task 
and just started the task from this function, using the xTaskCreate() function.

For the stack size, I thought 1024 would have been sufficient, but apparently 2048 was required. I want to get a better understanding of determining the
stack size in the future. Right now my only method is to raise it until it doesn't crash.

```
void blink_white_led()
{
    ESP_LOGI(TAG, "Turning the white LED %s!", s_white_led_state == true ? "ON" : "OFF");
    /* If the addressable LED is enabled */
    if (s_white_led_state)
    {
        /* Set the LED pixel using RGB from 0 (0%) to 255 (100%) for each color */
        led_strip_set_pixel(led_strip, 0, 5, 5, 5);
        /* Refresh the strip to send data */
        led_strip_refresh(led_strip);
    }
    else
    {
        /* Set all LED off to clear all pixels */
        led_strip_clear(led_strip);
    }
    /* Toggle the LED state */
    s_white_led_state = !s_white_led_state;
}

void blink_led_white_task(void *pvParameters)
{
    ESP_LOGI(TAG, "Starting the blink led white task!");
    while (1)
    {
        blink_white_led();
        vTaskDelay(1000 / portTICK_PERIOD_MS);
    }
}

void app_main(void)
{

    /* Configure the peripheral according to the LED type */
    configure_led();

    xTaskCreate(blink_led_white_task, "blink_led_white_task", 2048, NULL, 1, NULL);
}
```

I also found some extra code to add to app_main() to show me what tasks are running:
```
    /*
     * Print task list for debug */
    //  Buffer to hold task list (large enough to fit output)
    char task_list_buffer[2048];

    // Get and print task list
    printf("Task List:\n");
    printf("Name         State  Priority  Stack  Num\n");
    printf("-----------------------------------------\n");

    vTaskList(task_list_buffer);
    printf("%s\n", task_list_buffer);
```

This is a good debug feature for me to understand tasks for now. Obviously, we wouldn't want to keep this in production code.

At this point, I figured I should start experimenting with multiple tasks. I figured a logical next step would be to create another task 
that blinked the led another colour. I just picked blue:

```
void blink_blue_led()
{
    /* If the addressable LED is enabled */
    if (s_blue_led_state)
    {
        /* Set the LED pixel using RGB from 0 (0%) to 255 (100%) for each color */
        led_strip_set_pixel(led_strip, 0, 0, 0, 5);
        /* Refresh the strip to send data */
        led_strip_refresh(led_strip);
    }
    else
    {
        /* Set all LED off to clear all pixels */
        led_strip_clear(led_strip);
    }
    /* Toggle the LED state */
    s_blue_led_state = !s_blue_led_state;
}

void blink_led_blue_task(void *pvParameters)
{
    ESP_LOGI(TAG, "Starting the blink led blue task!");

    while (1)
    {
        ESP_LOGI(TAG, "Turning the blue LED %s!", s_blue_led_state == true ? "ON" : "OFF");
        blink_blue_led();
        vTaskDelay(1000 / portTICK_PERIOD_MS);
    }
}

void app_main(){
    ....
    xTaskCreate(blink_led_blue_task, "blink_led_blue_task", 2048, NULL, 1, NULL);
    ....
}
```

Unfortunately, I ran into an issue with concurrency immediately. I didn't spend too much time debugging, but I realized that the LED was blinking white,
then only flashing blue for a few microseconds before switching off completely. I realized that the issue was because the two tasks were sharing
the single LED resource. 

I knew from my studies that the way to control access to a shared resource was by using either a Mutex or a Semaphore, but I had no idea how to use either.
I decided to try asking Qwen to give me an example of how to integrate a Mutex in my code. Qwen actually suggested using a binary semaphore instead, since
it was supposedly simpler to use. I'll have to decide later (when I'm more experienced with these things) if it was correct.

For now, the binary semaphore worked perfectly! The basic idea that I had was that the led should blink a certain colour, pause, then blink that same 
colour, before switching over to the other colour. With the semaphore, I figured I should take the semaphore, blink the LED twice to ensure that the 
colour shows up, then give the semaphore back to the other colour. I'm sure that there were better ways to do this, but it worked! 

```
#include "freertos/semphr.h"
....
SemaphoreHandle_t led_semaphore;
....
void blink_led_white_task(void *pvParameters)
{
    ESP_LOGI(TAG, "Starting the blink led white task!");
    while (1)
    {
        xSemaphoreTake(led_semaphore, portMAX_DELAY);
        blink_white_led();
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        blink_white_led();
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        xSemaphoreGive(led_semaphore);
        vTaskDelay(2000 / portTICK_PERIOD_MS);
    }
}

void blink_led_blue_task(void *pvParameters)
{
    ESP_LOGI(TAG, "Starting the blink led blue task!");

    while (1)
    {
        ESP_LOGI(TAG, "Turning the blue LED %s!", s_blue_led_state == true ? "ON" : "OFF");
        xSemaphoreTake(led_semaphore, portMAX_DELAY);
        blink_blue_led();
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        blink_blue_led();
        vTaskDelay(1000 / portTICK_PERIOD_MS);
        xSemaphoreGive(led_semaphore);
        vTaskDelay(2000 / portTICK_PERIOD_MS);
    }
}
....
void app_main(void)
{
    // set up a semaphore to share the one RGB LED
    led_semaphore = xSemaphoreCreateBinary();
    if (led_semaphore == NULL)
    {
        ESP_LOGI(TAG, "Failed to create semaphore");
    }
    xSemaphoreGive(led_semaphore);
    /* Configure the peripheral according to the LED type */
    configure_led();
    xTaskCreate(blink_led_white_task, "blink_led_white_task", 2048, NULL, 1, NULL);
    xTaskCreate(blink_led_blue_task, "blink_led_blue_task", 2048, NULL, 1, NULL);
    ....
}

```

Once important point that I missed was that when you create your semaphore, you have to give it before it can be used. I thought it would be available 
by default.

This was a huge step for sure!

[![Final Solution](/assets/posts/2025-07-06-blinky_tasks_and_semaphores/final_solution.gif){: width="600" }](/assets/posts/2025-07-06-blinky_tasks_and_semaphores/final_solution.gif)
