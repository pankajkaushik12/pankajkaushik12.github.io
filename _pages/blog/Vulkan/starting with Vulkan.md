---
title: Starting with Vulkan
permalink: /starting-with-vulkan
layout: single
author_profile: false
---


Starting with Vulkan is easy. Below I will write the steps to build and render a triangle with Vulkan in Windows 11.

As a first step you needs to download the Vulkan sdk from ([LunarG](https://vulkan.lunarg.com/sdk/home#windows)) and follow the steps given in the tutorial their official ([tutorial](https://vulkan-tutorial.com/)).

The whole application is divided into 4 function:
- InitWindow
- InitVulkan
- MainLoop
- CleanUp

The above function are being called inside *Run()* function of the Application class, which itself is called from the *main()* function through an instance of the Application class.

### Handles
- *GLFWwindow\** Window: Window handle.
- *VkInstance* VulkanInstance: Vulkan handle.
- *VkPhysicalDevice* PhysicalDevice: GPU handle.

### **IntiWindow**
This create a window with *glfwCreateWindow* after intializing glfw with *glfwInit*.

### **InitVulkan**
Initializing Vulkan itself takes multiple steps.

-  Creating a Vulkan instance with *vkCreateInstance*, which takes 3 parameters:
    - Pointer to *VkInstanceCreateInfo* which we uses to define the properties like, set of extensions to be enabled (*ppEnabledExtensionNames*), set the names of validation layers to be enabled, *VkApplicationInfo*.
    - VkAllocationCallbacks which can be set to *nullptr* initially.
    - Reference to *VkInstance*.
- Creating a Surface *VkSurfaceKHR*, if you do want to visualize the rendering to a window. Surface handle can be created with *glfwCreateWindowSurface* that takes 4 parameter:
    - Reference to *VkInstance*.
    - Reference to *GLFWwindow\**.
    - Reference to VkAllocationCallbacks which can be set to nullptr for now.
    - Reference to *VkSurface*.
- Enumerate the physical devices and Select the physical device that is best for you. You can enumerate the physical devices list which you can get from *vkEnumeratePhysicalDevices* and check their individual properties (Supported queue families, Device extension support, etc.).
- Create a interface to the GPU with *VkDevice*. It needs the following:
    - Reference to the *VkPhysicalDevice*.
    - Reference to *VkDeviceCreateInfo*.
    - Reference to *VkAllocationCallbacks*.
    - Reference to *VkDevice*.
- After creating handle to *VkDevice*, we can get the queues associated with it by the *vkFetDeviceQueue* API call.
- Next to thing to do is to create a swapchain.