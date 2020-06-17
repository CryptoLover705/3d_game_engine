# CPP-3D-Engine
A project started in 2015 to overcome the boredom of the summer holidays. It uses a very similar
class-heirarchy system to that of Unreal Engine 4 and the Unity Game Engine, most notably the further
degree of abstraction between Entities and Components.

This is a work in development to say the least. I've never fully had the motivation to code something
substantial with this Engine. Yet it still exists as a demonstration of how much can be accomplished
without a game engine.

Compile using the GNU Compiler Collection (GCC).

# Dependencies:
- opengl
- glew
- glfw3
- glm
- openal
- freealut
- freeimage
- assimp

## Commandlines for compilation:
### Linux:
	make

### TODO (Non-exhaustive list):
- Game (Minor and virtually pointless):


### Changelog
- 22-8-15:
  - Started this changelog
  - Recoded the generation (for this) from the previously coded 2D engine to work with the 3D one
  - Added .cfg support and implemented .cfg WindowedResolution and Fullscreen
  - Realised windows MinGW g++ 4.8.1-4 doesn't support <regex> while trying to implement an obj file loader
- 23-8-15:
  - Implemented .obj support for models
  - Implemented Model precaching
- 4-9-15:
  - Implemented GLFW and removed SDL2
    - Forced to use MinGW-w64
      - Project is now 64-Bit
    - GLFW is much cleaner than SDL2
    - OpenGL 3.3+ compatability contexts now work. (wow!)
- 9-9-15:
  - Edited compile.py to execute make with -j(threads*1.5) for lightning fast compile times. (Sadly linking is still slow)
  - Edited makefile to use -pipe (apparently it improves the speed (or so stackoverflow says))
- 16-9-15:
  - Successfully added Precache functions for Models and Textures within the Renderer
  - ModelInfo and TextureInfo structs created to hold information about a particular model or texture stored within GPU memory
  - Successfully created CurrentTexture and CurrentModel hooks for the new class
  - Moved the SheetDemo into SheetDemoComponent.cpp/h
- 17-9-15:
  - Fixed OpenGL texture parameter on texture preloading to prevent the weird red lines from showing up
  - Added Model Normals to the GPU buffer objects. (For future lighting calculations)
- 10-10-15:
  - Coded a dynamic shader system with its own class. Shader.cpp
- 11-10-15:
  - Implemented OpenGL Vertex Array Objects
    - ShaderHelper.hpp is no longer a dependency of RendererOpenGL.cpp
- 20-10-15:
  - Ported to GNU/Linux (Newer Version of GCC)
- 8-11-15:
  - Implemented viewport size changes on screen size changes using a lambda function. (ty C++11)
- 14-11-15:
  - Fixed Rotation coordinate system. Conjugated rotation quaternion before passing it into the view matrix AND recoded the forward, right and up vectors in ITransform
  - Created a LookAt function in ITransform with an overload to specify an up vector to maintain upright after the change in direction (because Quaternions are horrible (In hindsight, they're much better than rotation matrices)
- 15-11-15:
  - Created a VolumeTimer class within Timer.h. This is a compact lambda function based timer. (Very useful)
- 29-11-15:
  - Implemented Assimp. Fixed normals and UVs
  - Implemented PlayerShip in Game as a Spaceship game thing
- 31-12-15:
  - Implemented generics/templates for Entity and Component creation in Entity.h. AddComponent<Component>() and Spawn<Entity>() now work
  - This AUTOMATICALLY typecasts the spawned class/component
  - Moved the public Engine* GameEngine variable inside Object.h rather than in Entity.h, Input.h, RendererOpenGL.h individually
  - It appears that Object.h now has a purpose. It controls code in ALL classes
  - Implemented GetComponent<Component>() and GetComponents<Component>() in Entity.h for easy component retrieval and interaction
  - Implemented PhysicsComponent.cpp/.h which hijacks the movement of the parent Entity. Implemented Linear Motion calculations with Mass and Velocity
  - PhysicsComponent->ApplyForce() applies a force to a PhysicsComponent
  - PhysicsComponent->Effector (poor variable naming?) By default it's the Owner. The PhysicsComponent can be owned and act upon different entities
  - PhysicsComponent also has MaximumSpeed and VelocityDamping variables to cap the speed and make things easier to control
  - CollisionComponent implemented. Only Sphere Collision works so far. Implemented object blocking in PlayerShip.cpp. (Ridiculous dot product vector mathematics and linear interpolation)
- 1-1-16:
  - Happy new year! Fixed lambda captures to capture the parent object. It turns out that [&] as a capture argument causes SegFaults
  - Contemplating implementing a PostBeginPlay method within classes to fix a nasty collision bug where the initial location of the component is at O (Fixed it by moving the collision detection code to after the entity list has been refreshed in the main loop)
- 2-1-16:
  - Coded basic raytracing. Static methods in CollisionComponent.h and referenced in Entity.h for convenience
  - All raytracing and collision related stuff has been moved to CollisionManager.h for convenience
    - Moved raytracing and the rest of the collision code to CollisionManager.h
  - Renamed Manager to GameEntityManager for clarity
- 2-1-16:
  - Implemented a ResourceManager. Moved rendering code to RenderComponent
- 16-1-16:
  - Fixed broken raytracing
  - Removed PhysicsComponent->Effector variable. (What's the point? Effector is the owner by default.
    - Why would a physics component induce physics on another object other than the owner? :P
- 4-2-16:
  - Fixed sphere collision deflection. No more collision leaks. It's built into the PhysicsComponent code.
- 13-2-16:
  - Moved component Render() before Tick() in the main loop. Parented components no longer lag behind.
- 16-2-16:
  - Implemented OpenAL. Sounds are now functional! :)
  - Sound.h/.cpp added
  - SoundManager.h/.cpp added
  - New Components:
    - SoundSourceComponent.h/.cpp
    - SoundListenerComponent.h/.cpp
- 17-2-16:
  - Added a DeflectionUtil namespace in the CollisionManager. :) Deflection is now built into the collision code
  - Removed many superfluous if(ptr == nullptr) unless it's truly necessary to explicitly state nullptr for the purpose of understanding
- 10-3-16:
  - Added a ShaderManager to improve the management of shaders. (is this really necessary?)
  - Unfortunately, shaders now require precaching. This includes materials. (This could be fixed later?)
  - I should port this back to windows, perhaps combine bin directories again
- 11-3-16:
  - alutLoadWAVFile() was deprecated. Updated WAV loading to the ALUT 1.1 CreateBufferFromWAVFile function
  - Implemented firing animations in PlayerShip.cpp using glm::clamp()
- 14-3-16:
  - Split textures and shaders. The same shader instance can be used across multiple entities (saves GPU memory).
- 8-12-17:
  - Fixed DemoSheetComponent.cpp (Coded a brilliant inheritance hack to render the sphere multiple times)
  - Forced OpenGL Core profile on linux, compatability causes a crash with modern glfw (3.2.1-1)
  - Switched to C++17 compilation
  - Recoded the linux build process to use pkg-config and a single Makefile
  - Recoded gen/* It was very poorly written
  - Removed the windows build process entirely (Hardly anyone uses MinGW32 on windows anyway. Little benefit in supporting Visual Studio)

