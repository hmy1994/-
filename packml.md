graph TD
    %% PackML状态模型基础
    subgraph PackML状态模型
        direction LR
        Initializing[初始化] --> Standby[待机]
        Standby --> Starting[启动中]
        Starting --> Execute[运行中]
        Execute --> Paused[暂停]
        Execute --> Stopping[停止中]
        Execute --> Fault[故障]
        Paused --> Execute
        Fault --> Standby
        Stopping --> Complete[完成]
    end

    %% 玻璃搬运系统具体流程
    subgraph 系统运行流程
        direction TB
        A[系统上电] --> B[PLC自检]
        B --> C[设备复位]
        C --> D[进入待机状态]
        D --> E{启动指令?}
        E -- 是 --> F[条件检查]
        F --> G[启动皮带1]
        G --> H[玻璃检测]
        H --> I{玻璃到位?}
        I -- 是 --> J[机器人抓取玻璃]
        J --> K[移动到皮带2位置]
        K --> L[放置玻璃到皮带2]
        L --> M[启动皮带2]
        M --> N[玻璃输送完成]
        N --> O{继续运行?}
        O -- 是 --> H
        O -- 否 --> P[停止流程]
        
        %% 异常处理分支
        H --> Q{异常检测?}
        Q -- 是 --> R[触发报警]
        R --> S[暂停设备]
        S --> T{故障排除?}
        T -- 是 --> H
        T -- 否 --> U[系统停机]
    end

    %% 系统组件交互
    subgraph 组件交互
        direction LR
        HMI[HMI界面] <--> PLC[PLC控制器]
        PLC <--> Robot[工业机器人]
        PLC <--> Belt1[皮带输送线1]
        PLC <--> Belt2[皮带输送线2]
        Belt1 <--> Sensor1[位置传感器1]
        Belt2 <--> Sensor2[位置传感器2]
        Sensor1 --> PLC
        Sensor2 --> PLC
    end

    %% 关键参数标注
    subgraph 技术参数
        direction BT
        RobotParam[机器人参数\n负载:5kg\n精度:±0.1mm]
        BeltParam[皮带参数\n速度:0-1m/s\n宽度:300mm]
        PLCParam[PLC参数\nI/O:128/64\n周期:<1ms]
    end
