#ifndef RVC_H
#define RVC_H
#include<stdbool.h>


/*
（一）Controller State  控制器状态  컨트롤러 상태
      enum成员全部大写。
*/
typedef enum{
    STATE_MOVE_FORWARD,  //forward
    STATE_MOVE_BACKWARD, //backward
    STATE_TURN_LEFT,     //turn left
    STATE_TURN_RIGHT,    //turn right
    STATE_POWER_UP       //turn on the power of the cleaner
} controller_state_t;

/*
（二）Obestacle Location  障碍物位置   장애물 위치
*/
typedef struct{
    bool front_obstacle;  //front obstacle
    bool left_obstacle;   //left obstacle
    bool right_obstacle;  //right obstacle
} obstacle_location_t;

/*
（三）Motor Command  电机命令   모터 명령
*/
typedef enum{
    MOTOR_FORWARD,   //forward
    MOTOR_BACKWARD,  //backward
    MOTOR_LEFT,      //turn left
    MOTOR_RIGHT      //turn right
} motor_command_t;

/*
(四)Direction 实际运动方向  실제 이동 방향
*/
typedef enum{
    DIRECTION_FORWARD,   //forward
    DIRECTION_BACKWARD,  //backward
    DIRECTION_LEFT,      //turn left
    DIRECTION_RIGHT      //turn right
} direction_t;

/*
（五）Cleaner Command  清洁命令   청소 명령
*/
typedef enum{
   CLEANER_OFF,      //turn off the cleaner
   CLEANER_ON,        //turn on the cleaner
   CLEANER_POWER_UP   //turn on the power of the cleaner
} cleaner_command_t;

/*
（六）Clean State  清洁器实际状态  청소기 실제 상태  
*/
typedef enum{
    CLEAN_OFF,      //turn off the cleaner
    CLEAN_ON,       //turn on the cleaner
    CLEAN_POWER_UP  //turn on the power of the cleaner
} clean_state_t;

/*
（七）Sensor Interface  传感器接口 센서 인터페이스
     ?助教今天说Sensor输入函数应该是无参数，通过返回值表示传感器状态。返回值为bool类型，true表示检测到障碍物/灰尘，false表示未检测到障碍物/灰尘。
     조교수님께서는 오늘 Sensor 입력 함수는 매개변수가 없어야 하며, 반환값으로 센서 상태를 나타낸다고 말씀하셨습니다. 
     반환값은 bool이며, true는 장애물/먼지가 감지되었음을 의미하고 false는 장애물/먼지가 감지되지 않았음을 의미합니다.
*/
bool front_sensor_interface(void); //front sensor interface
bool left_sensor_interface(void);  //left sensor interface
bool right_sensor_interface(void); //right sensor interface
bool dust_sensor_interface(void);  //dust sensor interface

/*
（八）Detection 检测灰尘位置  감지
*/
obstacle_location_t determine_obstacle_location(
    bool front_sensor,
    bool left_sensor,
    bool right_sensor
); 
/*
（九）Determine Dust Existence  判断灰尘是否存在
*/
bool determine_dust_existence(bool dust_detection); //dust if exist

/*
（十）Controller  控制器  컨트롤러
*/
void controller(
    obstacle_location_t obstacle_location,
    bool dust_existence,
    int tick
);

/*
（十一）Movement 移动控制  이동 제어
     括号里的参数是bool类型，表示是否触发/开启该动作。根据上一周作业的Level 3图。
*/
motor_command_t move_forward(bool enable); //forward
motor_command_t move_backward(bool trigger);//backward
motor_command_t turn_left(bool trigger);    //turn left
motor_command_t turn_right(bool trigger);   //turn right

cleaner_command_t power_up(bool trigger,int tick);  //turn on the power of the cleaner

/*
（十二）Interface Functions  接口功能  하드웨어 인터페이스
*/direction_t motor_interface(
    motor_command_t motor_command
);

 clean_state_t cleaner_interface(
    cleaner_command_t cleaner_command
);

#endif // RVC_H
