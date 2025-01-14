[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/MjLLqDcN)
# HW1
## W1L2 In-Class Activity

Objects: 
- UI 
    - seeds planted UI
      -  Attributes: text
      - Actions: count goes up when player plants seed
    - seeds remaining UI
      - Attributes: text
      - Actions: count goes down when player plants seed
- Player
    - Movement(Input-WASD and arrow keys; Output-player changing location)
    - Plant seeds: Input-Press space; Output-Set down 1 seed at player position IF player has a seed remaining; decreases seed count+increases plant count (shows in UI)
    - Attributes: bunny sprite
- Plants 
    - Attributes: plant sprite


## Devlog
Prompt: Include the HW1 break-down exercise you wrote during the Week 1 - Lecture 2 (Jan 9) in-class activity (above). If you did not attend and perform this activity, review the lecture slides and write your own plan for how you believe HW1 should be built. If your initially proposed plan turned out significantly different than the activity answers given by Prof Reid, you may want to note what was different. Then, write about how the plan you wrote in the break-down connects to the code you wrote. Cite specific class names and method names in the code and GameObjects in your Unity Scene.


I started by first writing the script for the player movement, for the WASD and arrow keys. Under the Update function in the Player script, I used an if statement, with the condition that (for example) when either the W key or up arrow is pressed, the player would move upward. (I changed the speed from 1 to 5.)
if (Input.GetKey(KeyCode.W) || Input.GetKey(KeyCode.UpArrow))
        {
            _playerTransform.Translate(Vector3.up*_speed*Time.deltaTime);
        }

Under the same Update function, I set up the if statement for planting seeds. If the spacebar is pressed AND the player has less than or equal to 5 seeds AND the number of seeds left is greater than 0, the PlantSeed function is called. 
if (Input.GetKeyDown(KeyCode.Space) && _numSeedsLeft<=5 && _numSeedsLeft>0)
        {
            PlantSeed();
        }

Before I wrote the PlantSeed function in the Player script, I went back to the Start function to set _numSeedsLeft equal to _numSeeds (the variable provided in the skeleton code), and _numSeedsPlanted to 0. These will be called in the PlantSeed function. 
private void Start ()
    {
        _numSeedsLeft=_numSeeds;
        _numSeedsPlanted=0;
    }

I added a plant sprite as a prefab in unity, then dragged the prefab from the project folder to the Plant Prefab space in the Player inspector to set it as a game object. Under the Canvas dropdown, I dragged the seeds planted and remaining text boxes to their respective places under the PlantCountUI script, then dragged the Canvas to the Plant Count UI place under the Player script in inspector. 

Then I was ready to write in the player’s action for planting the seeds. Under the PlantSeed function I instantiated the plant prefab to make it appear at the player’s location with Instantiate(prefab, player position, player orientation). I also subtracted the number of seeds left by 1 (--) and added the number of seeds planted by 1 (++). Finally, I called the PlantCountUI script with  _plantCountUI.UpdateSeeds(_numSeedsLeft, _numSeedsPlanted), in which _plantCountUI is the script’s name, UpdateSeeds is the function that is being called, and (_numSeedsLeft, _numSeedsPlanted) is the parameter corresponding to the variables in the PlantCountUI script with the same values but different names, (seedsLeft, seedsPlanted). 
public void PlantSeed()
    {
       Instantiate(_plantPrefab, _playerTransform.position, _playerTransform.rotation);
            _numSeedsLeft--;
            _numSeedsPlanted++;
            _plantCountUI.UpdateSeeds(_numSeedsLeft, _numSeedsPlanted);
    }

On Unity, I changed the “##” in the UI text boxes to 0 and 5 respectively, then went back to the UpdateSeeds function in the PlantCountUI script to convert the seedsPlanted and seedsLeft integers to strings that displayed in the Unity UI text boxes, _plantedText and _remainingText, respectively. 
public void UpdateSeeds (int seedsLeft, int seedsPlanted)
    {
        _plantedText.SetText(seedsPlanted.ToString());
        _remainingText.SetText(seedsLeft.ToString());
    }





## Open-Source Assets
If you added any other outside assets, list them here!
- [Sprout Lands sprite asset pack](https://cupnooble.itch.io/sprout-lands-asset-pack) - character and item sprites
