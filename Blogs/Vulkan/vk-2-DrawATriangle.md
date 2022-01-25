### 使用Vulkan画一个三角形[(*Source*)](https://vulkan-tutorial.com/Drawing_a_triangle/Setup/Base_code)

**主要内容**
- 画一个三角形

#### 
##### 画一个三角形
在画一个三角形中，我们使用类似如下的代码：
```cpp
#include <vulkan/vulkan.h>
#include <iostream>
#include <stdexcept>
#include <cstdlib>

class HelloTriangleApplication{
public:
    void run{
        initVulcan();
        mainLoop();
        cleanup();
    }

private:
    void initVulkan(){

    }

    void mainLoop(){

    }

    void cleanup(){

    }
}

int main(){
    HelloTriangleApplication app;

    try{
        app.run();
    }catch (const std::exception& e){
        std::ceer << e.what() << std::endl;
        return EXIT_FAILURE;
    }

    return EXIT_SUCCESS;
}
```

这个程序被包裹在一个类中，