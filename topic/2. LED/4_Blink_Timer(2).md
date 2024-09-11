# 4 Blink - Timer(2)
Code in Interrupt Handler

timer setting이 1초로 되어있는 상태에서 toggle을 3초마다 하고싶다면.

- [4 Blink - Timer(2)](#4-blink---timer2)
  - [Interrupt Handler](#interrupt-handler)
    - [stm32c0xx\_it.c](#stm32c0xx_itc)
    - [main.c](#mainc)

## Interrupt Handler
### stm32c0xx_it.c
```c
static uint32_t seconds_counter = 0;

void TIM14_IRQHandler(void)
{
    /* USER CODE BEGIN TIM14_IRQn 0 */
    if (__HAL_TIM_GET_FLAG(&htim14, TIM_FLAG_UPDATE) != RESET)
    {
        if (__HAL_TIM_GET_IT_SOURCE(&htim14, TIM_IT_UPDATE) != RESET)
        {
            __HAL_TIM_CLEAR_IT(&htim14, TIM_IT_UPDATE);

            // Increment the seconds counter
            seconds_counter++;

            // Check if 3 seconds have passed
            if (seconds_counter % 3 == 0)
            {
                BSP_LED_Toggle(LED_GREEN);
            }
        }
    }
    /* USER CODE END TIM14_IRQn 0 */
    HAL_TIM_IRQHandler(&htim14);
    /* USER CODE BEGIN TIM14_IRQn 1 */

    /* USER CODE END TIM14_IRQn 1 */
}
```
### main.c
```c
int main(void)
{

  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM14_Init();
  /* USER CODE BEGIN 2 */

  /* USER CODE END 2 */

  /* Initialize leds */
  BSP_LED_Init(LED_GREEN);

  /* Initialize USER push-button, will be used to trigger an interrupt each time it's pressed.*/
  BSP_PB_Init(BUTTON_USER, BUTTON_MODE_EXTI);

  /* Initialize COM1 port (115200, 8 bits (7-bit data + 1 stop bit), no parity */
  BspCOMInit.BaudRate   = 115200;
  BspCOMInit.WordLength = COM_WORDLENGTH_8B;
  BspCOMInit.StopBits   = COM_STOPBITS_1;
  BspCOMInit.Parity     = COM_PARITY_NONE;
  BspCOMInit.HwFlowCtl  = COM_HWCONTROL_NONE;
  if (BSP_COM_Init(COM1, &BspCOMInit) != BSP_ERROR_NONE)
  {
    Error_Handler();
  }

  if (HAL_TIM_Base_Start_IT(&htim14) != HAL_OK)
  {
	  Error_Handler();
  }

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {

    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}
```
