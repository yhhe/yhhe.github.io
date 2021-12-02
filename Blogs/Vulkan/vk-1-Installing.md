### 在MacOS上安装Vulkan[(*Source*)](https://vulkan-tutorial.com/Development_environment#page_MacOS)
----
#### 1. Vulkan SDK
在MacOS上可以使用的版本是MoltenVK。Macos不支持Vulkan，因此MoltenVK将Vulkan API转换为使用Metal图形框架。

#### 2. GLFW和GLM
GLFW用于创建窗口等。
GLM用于线性代数的计算。
```
brew install glfw
brew install glm
```

### 3. 设置Xcode
1. 创建Command Line Tool工程，语言选择C++
2. 在 Header Search Paths 中添加 /usr/local/include 和 vulkansdk/macOS/include
3. 在 Library Search Path 中添加 /usr/local/lib 和 vulkansdk/macOS/lib
4. 在 Link Binary With Libraries 添加 libglfw.3.x.dylib 和 libvulkan.1.dylib
5. 添加环境变量。在Product->Scheme->Edit Scheme->Arguments中增加环境变量
    - VK_ICD_FILENAMES = vulkansdk/macOS/share/vulkan/icd.d/MoltenVK_icd.json
    - VK_LAYER_PATH = vulkansdk/macOS/share/vulkan/explicit_layer.d

```cpp
// 测试用代码

#define GLFW_INCLUDE_VULKAN
#include <GLFW/glfw3.h>

#define GLM_FORCE_RADIANS
#define GLM_FORCE_DEPTH_ZERO_TO_ONE
#include <glm/vec4.hpp>
#include <glm/mat4x4.hpp>

#include <iostream>

int main() {
    glfwInit();

    glfwWindowHint(GLFW_CLIENT_API, GLFW_NO_API);
    GLFWwindow* window = glfwCreateWindow(800, 600, "Vulkan window", nullptr, nullptr);

    uint32_t extensionCount = 0;
    vkEnumerateInstanceExtensionProperties(nullptr, &extensionCount, nullptr);

    std::cout << extensionCount << " extensions supported\n";

    glm::mat4 matrix;
    glm::vec4 vec;
    auto test = matrix * vec;

    while(!glfwWindowShouldClose(window)) {
        glfwPollEvents();
    }

    glfwDestroyWindow(window);

    glfwTerminate();

    return 0;
}


```