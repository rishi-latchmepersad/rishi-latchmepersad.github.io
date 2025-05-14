---
title: GPIO on a Breadboard!
---

The GPIO on a breadboard turned out to be easier than I expected! I actually didn't use any videos for this. I got a bit brave, and I figured I could combine my knowledge
from the last couple of posts to get this part working.

I already had some basic knowledge of how to identify good ports on the board headers, so I searched online and found the mapping of the GPIO pins.

[![GPIO pins](/assets/posts/2025-05-12-basic_gpio_circuit/stm32_header_pin_mapping.PNG){: width="350" }](/assets/posts/2025-05-12-basic_gpio_circuit/stm32_header_pin_mapping.PNG)

I decided to go the easiest route and use the first pin on the header (A0/PA_3) for the LED, and the second pin (A1/PC_0) for the button.

I then modified the code slightly from my previous tutorials to use these pins:

    #include "main.h"
    #include "stdbool.h"

    int main(void) {
        // set up the GPIO A pins
        GPIOA3_Config();
        GPIOC0_Config();
        // create a variable to store the state of the button press
        bool flag = true;
        // run an infinite loop
        while (1) {
            // if the button is not being pressed and the variable is set
            if (HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_0) == 0 && flag) {
                // toggle the led
                HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_3);
                // reset the flag
                flag = false;
            }
            // else if the button is being pressed
            else if (HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_0) == 1) {
                flag = true;
            }
        }
    }

    // GPIOA configuration
    void GPIOA3_Config(void) {
        // enable the clock to bring the GPIO A connectors up from their low power default state
        __HAL_RCC_GPIOA_CLK_ENABLE();
        // initialize a struct to hold all the values for our GPIO device
        GPIO_InitTypeDef GPIOA_Init = { };
        // set the pin to 3
        GPIOA_Init.Pin = GPIO_PIN_3;
        // set the pin to the output push-pull mode
        GPIOA_Init.Mode = GPIO_MODE_OUTPUT_PP;
        // call the initialization function with this struct for GPIO A
        HAL_GPIO_Init(GPIOA, &GPIOA_Init);
    }

    void GPIOC0_Config(void) {
        // initialize a struct to hold all the values for our GPIO device
        __HAL_RCC_GPIOC_CLK_ENABLE();
        GPIO_InitTypeDef GPIOC_Init = { };
        // set the pin to 3
        GPIOC_Init.Pin = GPIO_PIN_0;
        // set the pin to the input mode
        GPIOC_Init.Mode = GPIO_MODE_INPUT;
        // call the initialization function with this struct for GPIO A
        HAL_GPIO_Init(GPIOC, &GPIOC_Init);
    }

For the circuit, I simply moved the jumper cables to these A0 and A1 pins.

[![GPIO circuit](/assets/posts/2025-05-12-basic_gpio_circuit/gpio_breadboard_button_circuit.jpg){: width="350" }](/assets/posts/2025-05-12-basic_gpio_circuit/gpio_breadboard_button_circuit.jpg)

I pushed the code, and it actually worked the first time! The LED was on by default. When I pushed the button, the flag got set. Then in the next iteration of the loop after the button was released, 
the pin was toggled and the LED was turned off, exactly as we expected!

I'm feeling a bit more confident now. I think the next stage of my experimentation has to be to get some debug output via the USB cable. I don't have an LCD screen yet,
so the only way I can read the output is through this USB interface :)