## Earliest Deadline First (EDF) Scheduler for FreeRTOS
This repository implements an Earliest Deadline First (EDF) dynamic priority scheduling algorithm within FreeRTOS. This project was developed for the Udacity Embedded Systems Advanced Nanodegree.
## Project Overview
FreeRTOS natively uses fixed-priority preemptive scheduling. This implementation modifies the core scheduler to support EDF, where tasks are scheduled dynamically based on their deadlines. The task with the nearest deadline always receives the highest execution priority.
## Key Features

* Dynamic priority assignment at runtime based on task deadlines.
* Preemptive scheduling when a task with an earlier deadline becomes ready.
* Missed deadline detection hook functions for system logging.

## Implementation Details## Modified FreeRTOS Files

* tasks.c: Custom logic added to the task selection mechanism.
* task.h: Extended Task Control Block (TCB) to store deadline variables.

## New API Functions

* vTaskCreateEDF(): Creates a periodic task with an explicit deadline.
* vTaskDelayUntilEDF(): Delays a task until its next execution window and updates its deadline.

## Usage Example

#include "FreeRTOS.h"#include "task.h"
void vPeriodicTask(void *pvParameters) {
    TickType_t xLastWakeTime = xTaskGetTickCount();
    const TickType_t xPeriod = pdMS_TO_TICKS(100);
    const TickType_t xDeadline = pdMS_TO_TICKS(80);

    for(;;) {
        // Task work execution
        
        // Relinquish CPU and update next deadline
        vTaskDelayUntilEDF(&xLastWakeTime, xPeriod, xDeadline);
    }
}
int main(void) {
    // Initialize hardware
    
    // Create EDF Task: Name, Stack Size, Parameter, Period, Deadline
    vTaskCreateEDF(vPeriodicTask, "Task1", 128, NULL, 100, 80);
    
    // Start Scheduler
    vTaskStartScheduler();
    for(;;);
}

## Verification and Testing

* Verified using logic analyzers and execution trace tools to confirm correct preemption.
* Validated against the EDF utilization bound condition where the total CPU utilization is less than or equal to 1.



