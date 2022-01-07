!!! tip "Before start"
    • Install programming environment by following tutorial on [Arduino setup](/setup).  
    • Join our community forum for any questions: [https://forum.totemmaker.net](https://forum.totemmaker.net){target=_blank}.  
!!! info "Command naming convention"  
    Some commands controls various channels (A, B, C, ...). There is a logic behind command naming for flexibility:  
    • `#!arduino write("commandA", 10)` - will set value `10` for individual channel A.  
    • `#!arduino write("commandAll", 100)` - will set value `100` for all existing channels.  
    • `#!arduino write("commandABCD", 10, 20, 30, 40)` - will set individual values for A, B, C, D channels.  
    • `#!arduino write("commandABC", 10, 20, 30)` - will set individual values for A, B, C channels.  
    Only commands listed in "Module commands" will be accepted. Other variations will be discarded.  
    All commands are case sensitive! Calls like `CommandA`, `COMMANDa`, `motorA/BRAKE` will be ignored.