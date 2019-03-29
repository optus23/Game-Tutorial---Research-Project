# Game Tutorial Design

## Why Tutorials has bad reputation?

Tutorials are probably one of the most important things in the experience of your game, and guess what... NOBODY CARES!! Well, that's not true at all, there is games like Half life2, Mario, Megman, Spiderman that cares a lot, but we are gonna speak about it later. 
The principal reason why a lot of companies fail desining Tutorials is because their Production Plan is basically centered in other things and are not taken into account at How Teach Player.

Look at this:
(place picture about popups)
(place picture about game controller)

This Pop-ups or Game Controller show-ups can't be in these games! But obviously there are a lot of games that are allowed to use this resource that are going to be explained later as well. 

## Teach Gradually Through Experience:
There are several questions that you must be able to answer about every mechanic of your game:
* What am I doing?
* How do I do it?
* Why do I do it?
* When do I do it?
Well, those four questions sound very simple but, there's a lot to explain and the player must think that information about every single action in a few milliseconds.




With the same reason, _Hotline Miami_ does not have to sort sprites. We can follow the order of sprites like this: background->furniture->enemies->guns->player.

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/hotline_miami.png?raw=true"/>

On the other hand, we have games like _The Legend of Zelda: Minish Cap_ and _Pokémon_ (from Ruby and Sapphire gba versions to Black and White versions) are a good example of the beginning of sorting sprites in video games.

In this example, I set player behind and front of that villager. We can see when the player is below the villager, the player overlaps the feet of the villager. And the same occurs when the player is above the villager.

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/Pokemon_Village_Example.png?raw=true"/>

Sprite ordering might be like this:

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/Sprite_ordering_gif.gif?raw=true"/>

## Camera Culling

Camera culling is a basic method that allows to the program to work with objects and entities that are only on the camera viewport. This is used to save resources to the machine. It helps especially in games with large worlds and a lot of entities to render. We will see the effect in program later.

# Resources

You can download the power point presentation [here](https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/raw/master/docs/Sprite_Ordering_and_Camera_culling.pptx) and you can download the project with a few exercises and the solution [here](https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research).

# Different approaches by different games

There are some systems to sorting sprites, it depends on the type of game, the resources of the machine and the code structure.

## Cut Sprites

This is the laziest way to solve the sorting sprites problem, but it can serve ample in many cases. It consists in separate a sprite in two parts, the down part and the high part. So, the core of the system is to render first the down part, later all entities, and finally the high part. That system is good to mix static and dynamic entities, for example a building isometric game. There is an example of _Pocket City_ made. It is quite interesting and fits well in that project for the simplicity of the project, the isometric type map and the mobile resources. You can see the separated layers and the result, tinted to see where the cut is.

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/PocketCity_layers.png?raw=true" width="400"/><img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/PocketCity_result.gif?raw=true" width="400"/>

## Sorting layers

This is the most common system used, but it could be used with different approaches.

### By position

That consist in sort entities depending of the position of an entity. It is only focus on the vertical position (Y). In order to make sense of depth, all entities and objects will be sorting by Y position, from low Y to high Y, from top of the window to down. Entities higher will render before entities below. We can see this example of _Chrono Trigger_ that uses this system.

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/chronoTrigger.png?raw=true" width="500"/>

That could consume more resources than we expected, because we must sort a lot of objects. In order to optimize, we have also to implement a camera culling, to only sort and render entities on camera. Also, it depends on the entity types and how are saved. The sort method also influences.

### Colliders

Sorting layers by colliders is not widely used but in some cases is the only way to get a good result. A good example for this, is a video made by [Guinxu](https://www.youtube.com/user/GuinxuVideos)(A Spanish indie game developer) that we can found [here](https://youtu.be/eMMnaUmWtnw?t=85)(In Spanish). In order to explain it, I will get some screenshots and I will explain how it works.

First, the problem is that the player will have to be able to pass under the bridge and to pass above. <img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/Bridge_Zelda_Example.png?raw=true" width="550"/>

To do that, Guinxu solved the problem putting up two types of colliders. One type made player be under bridge, and the other vice versa, so, when player goes over the bridge, the last collider that touches is the red (up arrow) and the player layer moves higher than bridge, when he comes out, the player touches blue collider (down arrow) and moves player layer below bridge. Also, that colliders with arrows active or deactivate colliders that let the player pass or not. For example, if player is going below bridge, he cannot be able to pass for the left and right like if he is passing above bridge, and the same case when player is going above bridge, he cannot be able to jump across bridge.
<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/Bridge_Zelda_Guinxu1.png?raw=true" width="560"/>
<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/Bridge_Zelda_Guinxu2.png?raw=true" width="560"/>

### Vector 3D

That is common in isometric map because it looks like a 3D environment. Also, is the most difficult way to implement. We must use 3 dimensions in order to get well the sorting of sprites. To render objects, we will have to project the vector3 to 2D. It is the most tricky and complex way to have sprite sorting. It also is the best way to sort sprites in isometric maps.

# Selected approach

In my case, I will sort layers by position, but with a modification. We won't operate with layers, we will operate directly with a list of entities. We will have an entity type that will be ```STATIC```, which will do nothing. I decided to do it like this because Tiled works only with objects with one tile, we will see what integration I have done from Tiled. Also, we will make a camera culling that prevent to render tiles that there are not on camera, sort entities and render that are only on the camera.

The result of this project that we want is something like this:

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/Result_Gif.gif?raw=true"/>

As we can see, player moves around objects and the program sorts the render order. On the title we can see the information of how many tiles are being rendered and how many entities are being sorted and rendered. In my case, I used a fictional pivot to sort entities, entities with pivot above will render before entities with pivot below.

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/debug_result.png?raw=true"/>

Pivot is the green rectangle in every entity.

## Profiling
We will see a radical change in the performancing of the program. There is a scene full of entities and tiles.

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/Profiling_scene.png?raw=true"/>

And now we will see the profiling of that scene during execution:

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/Profiling_high.png?raw=true"/>

We see that there are a lot of time wasted in render and sorting sprites. And now, here we have a profiling of the same scene but after making the research implementation:

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/Profiling_low.png?raw=true"/>

We can see that we gain like one and a half ms rendering background and 47 ms updating all entities. 17 ms still is a lot of time but we will se later how to optimize that. You can see more profiling tests in that folder: [Brofiling tests](https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/tree/master/docs/Brofiler_tests)

## Working with Tiled
In order to work with Tiled easily, I have implemented code to import entities to the game. I will explain how it works. That works from loading object layers in Tiled, you can see the code to load an entire map [here](https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/Solution/Motor2D/j1Map.cpp).

## Importing dynamic entities from Tiled

We can work with tilesets in Tiled. It allows us some functionalities. Only we must do is to study what it gives and incorporate to our code.
First, there is the main information of the tileset that we can see on Properties window.

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/player_tsx_properties.png?raw=true"/>

Here we have some general information about the tileset. The most important are:
* Name
* Orientation
* Columns
* Image
  * Source
  * Tile Width
  * Tile Height
  * Margin
  * Spacing

All these variables will be important to import to the program. This is something very important: Custom Properties. There we can assign every variable we want to the code and edit so fast. In my example I use AnimationSpeed but it can be used to many things. It can be of different types: bool, float, int, string...

Also is a powerful tool to implement animations easily. All we have to do is pick the camera icon, set a reference tile and drag it to the box to set the animation of an action. Each tile has an id that we will use later to assign the animation.

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/player_tsx.png?raw=true"/>

We can also set many colliders and load after in code, but it won't affect to the research, so we won't touch that utility.

After we save the file, we will get something like that:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<tileset version="1.2" tiledversion="1.2.2" name="Player" tilewidth="14" tileheight="21" tilecount="9" columns="3">
 <properties>
  <property name="AnimationSpeed" type="float" value="5"/>
 </properties>
 <image source="Player.png" width="42" height="63"/>
 <tile id="0">
  <animation>
   <frame tileid="0" duration="200"/>
  </animation>
 </tile>
 <tile id="3">
  <animation>
   <frame tileid="0" duration="200"/>
   <frame tileid="3" duration="200"/>
   <frame tileid="0" duration="200"/>
   <frame tileid="6" duration="200"/>
  </animation>
 </tile>
</tileset>
```

Here we have in a XML the general information, properties and animations.

Now, in the code we will create some structs to save the data. We will create a Entity class and then all entities than has an special behaviour will inherited from it.

```cpp
struct EntityInfo {
	TileSetEntity tileset;
	EntityAnim* animations = nullptr;
	uint num_animations = 0;
};
```

We will create a basic structure that saves the tileset of the entity and the animations.

```cpp
struct TileSetEntity {

	SDL_Rect GetTileRect(int id) const;

	std::string name;
	uint tilewidth = 0;
	uint tileheight = 0;
	uint spacing = 0;
	uint margin = 0;
	uint tilecount = 0;
	uint columns = 0;
	std::string imagePath;
	SDL_Texture* texture = nullptr;
	uint width = 0;
	uint height = 0;
};
```

For the tileset we will save some information to load the texture. ```SDL_Rect GetTileRect(int id) const;``` returns a rect given an id. For example, id 0 is the first tile, the player looking down, so, will fill the rect with the tileset information given the width and height, and the position where is in the texture.

```cpp
//Get the rect info of an id of tileset
SDL_Rect TileSetEntity::GetTileRect(int id) const {
	SDL_Rect rect;
	rect.w = tilewidth;
	rect.h = tileheight;
	rect.x = margin + ((rect.w + spacing) * (id % columns));
	rect.y = margin + ((rect.h + spacing) * (id / columns));
	return rect;
}
```

On animations we will save the id where is come from, number of frames, the position of frames on the texture and the type of the animation. ```uint FrameCount();``` is a simple function that iterates all xml nodes and return how many frames animation has.

```cpp
struct EntityAnim {
	uint id = 0;
	uint num_frames = 0;
	SDL_Rect* frames = nullptr;
	EntityState animType;

	uint FrameCount(pugi::xml_node&);
};
```

We won't use colliders, but I will give the struct to save information. We must save the collider, the offset from the entity position, the size and the type.

```cpp
struct COLLIDER_INFO {
	Collider* collider = nullptr;
	iPoint offset;
	int width = 0;
	int height = 0;
	COLLIDER_TYPE type;
};
```

In order to load all this information, we will have some functions, some of that will be virtual because every entity will have its animations and properties.

```cpp
bool LoadEntityData(const char*); //Loads entity by tsx file
	//Virtual functions because every entity has its properties, variables, animations...------------------------
	virtual void LoadProperties(pugi::xml_node&);
	virtual void LoadCollider(pugi::xml_node&);
	virtual void IdAnimToEnum();
	virtual void PushBack() {};
	virtual void AddColliders(j1Entity* c = nullptr);
	//-----------------------------------------------------------------------------------------------------------
```

```LoadEntityData()``` will contain all others functions. First, we save tile set information:

```cpp
//fill tileset info
	pugi::xml_node node = entity_file.child("tileset");

	data.tileset.name.assign(node.attribute("name").as_string());
	data.tileset.tilewidth = node.attribute("tilewidth").as_uint();
	data.tileset.tileheight = node.attribute("tileheight").as_uint();
	data.tileset.spacing = node.attribute("spacing").as_uint();
	data.tileset.margin = node.attribute("margin").as_uint();
	data.tileset.tilecount = node.attribute("tilecount").as_uint();
	data.tileset.columns = node.attribute("columns").as_uint();
	data.tileset.imagePath = folder += node.child("image").attribute("source").as_string();
	data.tileset.width = node.child("image").attribute("width").as_uint();
	data.tileset.height = node.child("image").attribute("height").as_uint();

	size = iPoint(data.tileset.tilewidth, data.tileset.tileheight);
```

Later we count how many animations there are and reserve memory for all of them.
```cpp
	//count how many animations are in file
	node = node.child("tile");
	data.num_animations = 0;
	while (node != NULL) {
		data.num_animations++;
		node = node.next_sibling("tile");
	}

	//reserve memory for all animations
	data.animations = new EntityAnim[data.num_animations];
```

Now, we want to save frames of animations.

```cpp
//count how many frames for each animation, assign memory for those frames and set id frame start
	node = entity_file.child("tileset").child("tile");
	for (uint i = 0; i < data.num_animations; ++i) {
		data.animations[i].FrameCount(node.child("animation").child("frame"));
		data.animations[i].frames = new SDL_Rect[data.animations[i].num_frames];
		data.animations[i].id = node.attribute("id").as_uint();
		node = node.next_sibling("tile");
	}

	//fill frame array with current information
	node = entity_file.child("tileset").child("tile");
	pugi::xml_node node_frame;
	for (uint i = 0; i < data.num_animations; ++i) {
		node_frame = node.child("animation").child("frame");
		for (uint j = 0; j < data.animations[i].num_frames; ++j) {
			data.animations[i].frames[j] = data.tileset.GetTileRect(node_frame.attribute("tileid").as_uint());
			node_frame = node_frame.next_sibling("frame");
		}
		node = node.next_sibling("tile");
	}
```

Later we save properties.

```cpp
LoadProperties(entity_file.child("tileset").child("properties").child("property")); //Load properties, is a virtual function because every entity has its variables
```
An example of Load properties is:
```cpp
void Player::LoadProperties(pugi::xml_node &node)
{
	std::string nameIdentificator;
	while (node) {
		nameIdentificator = node.attribute("name").as_string();

		if (nameIdentificator == "AnimationSpeed")
			animationSpeed = node.attribute("value").as_float();

		node = node.next_sibling();
	}
}
```
Later Load collider.
```cpp
LoadCollider(entity_file.child("tileset").child("tile").child("objectgroup").child("object")); //Load collider
```
And there is an example:
```cpp
void Player::LoadCollider(pugi::xml_node &node)
{
	std::string nameIdentificator;
	while (node) {
		nameIdentificator = node.attribute("name").as_string();

		if (nameIdentificator == "Collider") {
			collider.offset.x = node.attribute("x").as_int();
			collider.offset.y = node.attribute("y").as_int();
			collider.width = node.attribute("width").as_uint();
			collider.height = node.attribute("height").as_uint();
			collider.type = COLLIDER_TYPE::COLLIDER_PLAYER;
		}

		node = node.next_sibling();
	}
}
```
Now, we must convert id animations to enum animations. To do that we use a virtual function. An example may be:
```cpp
void Player::IdAnimToEnum()
{
	for (uint i = 0; i < data.num_animations; ++i) {
		switch (data.animations[i].id) {
		case 0:
			data.animations[i].animType = EntityState::IDLE;
			break;
		case 3:
			data.animations[i].animType = EntityState::WALKING;
			break;
		default:
			data.animations[i].animType = EntityState::UNKNOWN;
			break;
		}
	}
}
```
After getting all animations, we must make the pushback of the frames. An example:
```cpp
void Player::PushBack() {

	for (uint i = 0; i < data.num_animations; ++i) {
		for (uint j = 0; j < data.animations[i].num_frames; ++j) {
			switch (data.animations[i].animType) {
			case EntityState::IDLE:
				anim_idle.PushBack(data.animations[i].frames[j]);
				break;
			case EntityState::WALKING:
				anim_walking.PushBack(data.animations[i].frames[j]);
				break;
			default:
				break;
			}
		}
	}
}
```

To finish, we have to delete all reserved memory that we won't use.
```cpp
//deleting entity animation data already loaded in its corresponding animation variables
	for (uint i = 0; i < data.num_animations; ++i) {		//this block of code delete animation data loaded of xml,
		if (data.animations[i].frames != nullptr) {			//is in PushBack() because when load all animation in its
			delete[] data.animations[i].frames;				//corresponding variables, that data is useless
			data.animations[i].frames = nullptr;
		}
	}
	if (data.animations != nullptr) {
		delete[] data.animations;
		data.animations = nullptr;
	}
```

With that we have finished the load of an entity with Tiled. You can find the code of load entity [here](https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/Solution/Motor2D/j1Entity.h) for the header and [here](https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/Solution/Motor2D/j1Entity.cpp) for the .cpp.

## Importing static entities from Tiled

For static entities it is a little different. It could not be that automatic. But it is not difficult.

First, we must prepare the scene. We will work with three layers.

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/layers.PNG?raw=true"/>

Background will contain all tiles that won't be affected by entities, the basic ground like grass and inaccessible trees. Now, the "Object" layer is useful to see where the objects on the scene will be. All objects will have to be in a single texture, working with an atlas texture of objects. It is so important to have the property ```NoDraw``` in off to don't render it later. Finally, we have a layer called ```StaticObjects``` and here we will set all objects in scene. Here is an example of putting a tree on scene:

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/tree_example.PNG?raw=true"/>

As we can see, we have to put the name and set the type to "static". Also we have to fill all tiles that occupies. Now we can pass to code.

We will create an entity, setting its position and name. Depending of the name we will assign a rect or another for the texture.
```cpp
ent_Static::ent_Static(int x, int y, std::string name) :j1Entity(Types::STATIC, x, y, name)
{
	//assign type of static entity, texture rect and pivot
	//Orthogonal map ------------------------
	if (name == "tree") {
		type = ent_Static::Type::TREE;
		SetRect(16, 0, 32, 48);
		SetPivot(15, 36);
	}
	else if (name == "statue") {
		type = ent_Static::Type::STATUE;
		SetRect(0, 48, 112, 160);
		SetPivot(60, 140);
	}
	else if (name == "house") {
		type = ent_Static::Type::HOUSE;
		SetRect(128, 0, 80, 96);
		SetPivot(40, 94);
	}
	else {
		LOG("There isn't any type assigned to %s name entity", name.data());
	}
	
	size = iPoint(frame.w, frame.h);
	
	data.tileset.texture = App->tex->Load(App->map->data.properties.objects_path.data()); //Load object texture
```

And that is all. To render we will give the texture and the frame we have loaded. Remember that it is only useful if the object will do nothing about interaction, only will be there.

You can download the release [here](https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/releases/tag/1.5).
And if you want the code, you can get it [here](https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research).

# Links to more documentation

If you want to know more about Sprite Ordering, here you have some links of interest:

* [Cut Sprites solution](https://trederia.blogspot.com/2014/08/z-ordering-of-sprites-in-tiled-maps.html)
* [Different approaches to Ordering by position](https://eliasdaler.wordpress.com/2013/11/20/z-order-in-top-down-2d-games/)
* [Sprite ordering and tiles](https://love2d.org/forums/viewtopic.php?t=77149)
* [Pocket City solution](https://blog.pocketcitygame.com/cheating-at-z-depth-sprite-sorting-in-an-isometric-game/amp/)

In that case, I will separate links in two sections, because sorting in isometric view could be tricky in some cases:

* [Isometric with objects with more than one tile](https://gamedev.stackexchange.com/questions/103442/how-do-i-determine-the-draw-order-of-isometric-2d-objects-occupying-multiple-til)
* [Isometric sprite ordering with Tiled](https://discourse.mapeditor.org/t/isometric-depth-sorting/736)
* [Drawing isometric boxes in the correct order](https://shaunlebron.github.io/IsometricBlocks/)
* [Isometric depth sorting with big objects](https://stackoverflow.com/questions/11166667/isometric-depth-sorting-issue-with-big-objects)
* [Optimization of sorting sprites in isometric](https://gamedev.stackexchange.com/questions/97442/sorting-sprites-layers-in-isometric-view)
* [Sort with Z-buffer and anchor point, isometric map](https://gamedev.stackexchange.com/questions/69851/isometric-map-draw-sort-with-z-buffer-and-anchor-point)
* [Isometric sort algorithm](https://gamedev.stackexchange.com/questions/8151/how-do-i-sort-isometric-sprites-into-the-correct-order)

# TODOs and Solution

## TODO 1: Create IsOnCamera function

### Explication
You have to pass to the function a rectangle to determine if it is on camera or not. It is quite important to pass a rectangle, not only a position because tiles and objects have a width and a height. The functionating of the camera could be different, in my case I had to put it negative position to work well. Also, if there is something related to the scale of the pixels you have to put it to be able to know if it is real on camera or not. In my case I am using SDL and that library has a function to know if two rectangles are or not intersecting, ```SDL_HasIntersection```.

### Test
You cannot test if it works until the next TODO.

## TODO 2: Use previous function to only render tiles on camera

### Explication
Now it is the time to know if it works. You have to pass the position of the tile in pixels and the width and the height of the tiles of the map.

### Test
You can test it moving camera in all directions and looking if the Tile count on the title changes.

## TODO 3: Create a post on Tiled and integrate in code

<img src="https://github.com/christt105/Sprite_Ordering_and_Camera_Culling_Personal_Research/blob/master/docs/web_images/post.PNG?raw=true"/>

### Explication
You must follow the steps we have explained above this to create a static entity. In that case we will create a post. Is quite simple, first put it on Tiled and later follow the same structure that other objects.

### Test
In theory, if it has been well done, it will be where you put on the map. If is not, look carefully the steps and see if there is any LOG message.

## TODO 4: Save entities on camera during update iteration in draw_entities vector and iterate after update iteration

### Explication
We will start sprite ordering area. You must move draw functions to another iteration. You have a vector called ```draw_entities``` that you must use to iterate. During Update() iteration you have to push back entities on camera. Later, iterating that vector, you will have to draw entities.

### Test
You can move the camera out of the map and see if the entity count is 0 or not.

## TODO 5: Use std::sort(Iterator first, Iterator last, Compare comp) before iterate draw_entities.

### Explication
Once you have only entities on camera, you must sort them in order to render before entities above. To do that there is a function in ```<algorithm>``` library to sort vectors. The function is sort() and you have to pass the beginning and the end of a vector. For that case we also have to pass a function, it is created on ```j1EntityManager::SortByYPos``` and sorts the position of an entity.

### Test
There will be some buildings and trees that will be correctly sorted. You can also move the player for the scene and see if it is sorting by the position.

## TODO 6: Add the pivot position to general position

### Explication
Sorting sprites by y position could be enough, but if we set the point where it will be the sorting will be much better. Just add the pivot y position to its entity.

### Test
Moving player will sort better some objects.

## Solutions

### TODO 1
```cpp
bool j1Render::IsOnCamera(const int & x, const int & y, const int & w, const int & h) const
{
	int scale = App->win->GetScale();

	SDL_Rect r = { x*scale,y*scale,w*scale,h*scale };
	SDL_Rect cam = { -camera.x,-camera.y,camera.w,camera.h };

	return SDL_HasIntersection(&r,&cam);
}
```

### TODO 2

```cpp
if (App->render->IsOnCamera(MapToWorld(i, j).x, MapToWorld(i, j).y, data.tile_width, data.tile_height)) {
```

### TODO 3

```cpp
else if (name == "post") {
		type = ent_Static::Type::POST;
		SetRect(0, 0, 16, 32);
		SetPivot(8, 15);
	}
```


### TODO 4
```cpp
for (std::vector<j1Entity*>::iterator item = entities.begin();item != entities.end(); ++item) {
		if (*item != nullptr) {
			ret = (*item)->Update(dt);
			
			if (App->render->IsOnCamera((*item)->position.x, (*item)->position.y, (*item)->size.x, (*item)->size.y)) {
				draw_entities.push_back(*item);
			}
		}
	}
	
for (std::vector<j1Entity*>::iterator item = draw_entities.begin(); item != draw_entities.end(); ++item) {
		(*item)->Draw();
		entities_drawn++;

		if (App->scene->entities_box) {
			DrawDebugQuad(*item);
		}
	}
```
### TODO 5
```cpp
std::sort(draw_entities.begin(), draw_entities.end(), j1EntityManager::SortByYPos);
```
### TODO 6
```cpp
static bool SortByYPos(const j1Entity * ent1, const j1Entity * ent2)
	{
		return ent1->pivot.y + ent1->position.y < ent2->pivot.y + ent2->position.y;
	}
```

# Issues

* Objects in isometric with more than one tile of size will have problems to order. In many cases, the best thing to do is to separate objects in one by one tile.

# Improvements

* Loading textures is so expensive and now we have a texture loaded for every entity. A possible solution is to have only the textures we will use, and set by an id the texture that you need. For example texture 0 to background, 1 to player, 2 to objects and 3 to npc's.
* There are a lot of sorting methods and a lot of type of containers. Sorting vectors doing sort() function is expensive. You have to study which container and sorting method adjust better to your game. Here you have some links to different types of sorting methods.
[Brilliant.org](https://brilliant.org/wiki/sorting-algorithms/)
[Geeksforgeeks.org](https://www.geeksforgeeks.org/sorting-algorithms/)
* Iterate all map and know if that tile is on camera is so expensive when map is so big. In order to improve that, you can iterate the map from the beginning of the camera position to the camera position plus viewport size.
* The way that we implement static entities is simple but it is not automatic and it is ugly to see hardcode. First improve is to set all static entities information in a XML file. Second, you can investigate a way to load pivot and frame of static entities in Tiled.

# Author

All the project has been done by Christian Martínez de la Rosa. You can find me in:
* Github: [christt105](https://github.com/christt105)
* Contact: christt105@gmail.com
