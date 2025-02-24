# Task 5

Project Title:
"2-Bit Digital Comparator with Visual Feedback on VSDSquadron Mini Board"

Project Overview:
This project is focused on implementing a 2-bit comparator using the VSDSquadron Mini board, where two 2-bit binary numbers are compared, and their relationship (greater than, equal to, or less than) is visually represented using LEDs. The comparator logic is coded in C using Visual Studio Code and is tested on hardware by connecting the board to a breadboard setup. The comparison results are displayed through three LEDs: Yellow for greater than, Red for less than, and Green for equal to. This project aims to combine digital circuit design, C programming, and hands-on hardware interfacing for a fun and educational experience.

Project Objective:
The primary goal of this project is to:

Design and implement a 2-bit comparator in C programming.
Implement the design on the VSDSquadron Mini board.
Verify functionality using LEDs to visually represent comparison outcomes.
Gain practical knowledge in digital circuit design and embedded programming.
Key Components:
VSDSquadron Mini Board: This is the core microcontroller used for processing and logic execution.
Breadboard & Jumper Wires: To build and connect the circuit for the LEDs.
LEDs (3): Each LED is assigned a specific role to visually indicate the result of the comparison.
Resistors (220Ω): Used to limit current and protect the LEDs.
Circuit Configuration:
LED1 (Yellow): Connected to Pin 4 (PD4) of the VSDSquadron Mini. Lights up if a > b.
LED2 (Red): Connected to Pin 5 (PD5) of the VSDSquadron Mini. Lights up if a == b.
LED3 (Green): Connected to Pin 6 (PD6) of the VSDSquadron Mini. Lights up if a < b.
Truth Table of the 2-Bit Comparator:
A1	A0	B1	B0	A > B	A = B	A < B
0	0	0	0	0	1	0
0	0	0	1	0	0	1
0	0	1	0	0	0	1
0	0	1	1	0	0	1
0	1	0	0	1	0	0
0	1	0	1	0	1	0
0	1	1	0	0	0	1
0	1	1	1	0	0	1
1	0	0	0	1	0	0
1	0	0	1	1	0	0
1	0	1	0	0	1	0
1	0	1	1	0	0	1
1	1	0	0	1	0	0
1	1	0	1	1	0	0
1	1	1	0	1	0	0
1	1	1	1	0	1	0
Code Explanation:
The implementation is written in C for the VSDSquadron Mini Board. The code uses GPIO ports to control the LEDs, which are mapped to specific pins (PD4, PD5, and PD6). Based on the comparison between two 2-bit numbers a and b, the corresponding LED is turned on to indicate the result (greater than, equal to, or less than).

The compare_2bit() function compares two numbers and sets the appropriate LED. The main loop iterates over all possible 2-bit values for both numbers and visualizes the result by toggling the LEDs accordingly.

Code Overview:
c
Copy code
#include <ch32v00x.h>
#include <debug.h>
#include<stdio.h>

#define LED1_PIN GPIO_Pin_4 // yellow LED
#define LED2_PIN GPIO_Pin_5 // red LED
#define LED3_PIN GPIO_Pin_6 // green LED
#define LED_PORT GPIOD

void GPIO_Config(void) {
    // Enable clock for GPIOD
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Pin = LED1_PIN | LED2_PIN | LED3_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(LED_PORT, &GPIO_InitStructure);
}

void compare_2bit(uint8_t a, uint8_t b) {
    // Clear LEDs
    GPIO_ResetBits(LED_PORT, LED1_PIN | LED2_PIN | LED3_PIN);
    if (a > b) {
        GPIO_SetBits(LED_PORT, LED1_PIN); // LED1 for a > b
    } else if (a == b) {
        GPIO_SetBits(LED_PORT, LED2_PIN); // LED2 for a == b
    } else {
        GPIO_SetBits(LED_PORT, LED3_PIN); // LED3 for a < b
    }
}

int main(void) {
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
    SystemCoreClockUpdate();
    Delay_Init();
    GPIO_Config();
    
    // Test all possible 2-bit numbers
    for (uint8_t a = 0; a <= 3; a++) {
        for (uint8_t b = 0; b <= 3; b++) {
            compare_2bit(a, b);
            Delay_Ms(5000); // Delay for 5 seconds
        }
    }
    
    return 0;
}
Conclusion:
This project demonstrates a simple and effective way of comparing two 2-bit numbers using the VSDSquadron Mini board. It leverages basic digital logic, GPIO control, and C programming to provide a clear visual output using LEDs. The project offers a great opportunity to practice embedded systems programming, digital logic design, and hardware interfacing.
