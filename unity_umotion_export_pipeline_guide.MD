# Migrating Characters and Animations from Unity to Flax using UMotion pro

> Disclaimer: I'm no blender nor animations expert, just a programmer moving functional characters and animations from Unity to Flax engine, so it's possible there are better and more professional ways to make this work.
>
> I'm just describing the steps I follow to make them work.

### Situation:
I'm trying to make Synty characters work with animations that were not made for them (in this tutorial I'll use Protofactor's animations as an example).

While Flax has a built-in animation retargeting tool, Synty's model structure doesn't work well with it and the models remain static despite the animation being played.

**This doesn't happen with Mixamo characters so I blame Synty.**

In this guide I'll be using characters from Synty's City Characters pack, and animations from Protofactor's Ultimate Animation Collection pack.

### Requirements:
* Unity
* UMotion pro
* Blender or your 3d modeling software of choice
* Some .fbx animation asset(s) that work in Unity
* A character model

* Flax engine (this doc is using Flax 1.6)


## 1. Blender: Remove unwanted characters from the main characters fbx pack

Synty tends to put multiple characters under the same fbx file, usually named `Characters.fbx`, we need to remove the characters we don't need and only leave **the chosen one**

#### 1.1 Import character

First, open an empty scene in blender and import synty's fbx file with the characters. In the case of Synty's City Characters (and many other of their assets) this file is named `Characters.fbx` and is usually located at `./Source_Files/FBX` from the package root.

Delete the default cube, camera and light from blender, only the character should be at the scene.

![Character f*ckd up][def]

If your character looks like this, change to pose mode then go to `Pose > Clear Transform` and click `All`. It should correct the transform.

![Fixed transform][def2]

#### 1.2 Delete unwanted characters

Now, open the characters armature and delete every character that you don't need, in this guide I'll use the Road Worker.

> place holder 3_

![Before unwanted character deletion][def_before_deletion]

Before

> place holder 4_

![After deletion][def_4_after_deletion]

After

#### 1.3 Export the character to Fbx
Export the character to a location of your choice and make a copy of that export. We will use this copy as a target for the animation with UMotion.

> place holder 5_

![Export options][def_5_export_options]

I'm using this options for export, don't know don't care what they do, *it just works* ®

I exported the character as `synty_roadworker.fbx` and created a copy as `synty_roadworker_anim.fbx`

### 2. Export animation with UMotion PRO

Now we are going to export the desired animation.

#### 2.1 Move FBX files into the unity project
Simple as, move the 2 fbx files from the previous section into Unity. Check that they are correctly set as humanoid.

Also, if you haven't already, import the desired animation fbx file.

#### 2.2 Configure project and character with UMotion PRO

Now, open UMotion PRO clip and create a new humanoid UMotion PRO project. For simplicity, I placed the fbx files and the umotion project file inside the same folder.

Once the project is created, drag the character into the scene or hierarchy to create an instance of it (it should automatically add an Animator component)

Open UMotion Pose editor drag (or select) the character inside the `Select a GameObject to animate` field, if done correclty, it should look like this:

> place holder 6_

![Pose editor options][def_6_pose_editor_options]


#### 2.3 Prepare animation for export

Now, go back to the Clip Editor, if everything has been done correctly so far, you should see this on the left panel:

> place holder 7_

![Clip editor first step][def_7_clip_editor_first_step]

Go to `File > Import Clips` and select the desired animation

> place holder 8_

![Clip editor window][def_8_clip_editor_window]


I'm adding the Sprint Forward Unarmed animation. The yellow warning indicates that some transforms from the animation don't exist in the model, you can ignore this (I think, have you read the disclaimer at the top? :P )

Hit import, if everything goes well, you should see the keyframes show up in UMotion's Clip Editor.

> place holder 9_

![Clip editor success!][def_9_clip_editor_success]

You can hit play in Clip Editor and should see the character being animated in the scene

> place holder 10_

![Character being animated in UMotion][def_10_character_animated_umotion]

#### 2.3 Export the animation

We're almost there! In UMotion Clip Editor go to `File > Export > Current Clip` (check that the current clip is the one you want!)

You may see a message like this:

> place holder 11_

![No export destination message][def_11_no_export_dest_message]

Don't worry! Click Open Settings. The Clip Export Settings window should open.
Configure as I did here:

> place holder 12_

![Clip export settings][def_12_clip_export_settings]

Set File format as Fbx Binary
Write mode to Update Existing File

And set destination file to the copy of the .fbx file you imported into the project, in my case it's `synty_roadworker_anim.fbx`.

> So remember, one .fbx file holds the character, and the other one holds the animation!

Some other configurations may work, I don't know, these are just that worked for me.

Hit close once you're done.
Now, again, in UMotion Clip Editor go to` File > Export > Current Clip`. UMotion will start exporting the clip.

If everything goes right, you should see a message like this:

> place holder 13_

![Export success!][def_13_clip_export_success]

Export is done!

### 3. Import character and animations into Flax

Standard procedure here. You can refer to Flax official docs for details.

Import both the character .fbx file and the animation .fbx file from UMotion PRO into Flax (you can drag and drop from Unity editor).

Remember to set the character as a `Skinned Model` and the Animation as `Animation` when you import them into Flax!

#### 3.1 Possible problem with materials


You may see that the character you just imported is missing it's material and look all gray. This seems to happen (my guess as an absolute ignorant in the field of technical art) because how Synty handles material atlases.

It's very easy to fix tho, don't worry, you can either create the material manually using the textures that should be present in your asset pack, or do the following:

* Go to Synty asset folder and import into Flax the same .fbx file we imported into blender (or any other file that carry the same atlas)
* Import that original Character.fbx file as a `Skinned Model` as we did with our character.
* You should see that once the import is done, a folder should show up with the material at the same location where Character.fbx file is (folder name should be Character too!)
* Inside that folder, is the material atlas, open your character (roadworker for example) by double clicking on it, go to materials and select the material from the menu.
* It should now show the correct materials! (also you can safely delete the original Character.fbx file from the flax project, as we already have the material atlas setup)

> place holder 14_

![Material correction][def_14_material_correction]

Great!

#### 3.2 Finally!

Now double click on the animation we imported (in this guide: `synty_roadworker_anim.fbx`), select the character (`synty_roadworker.fbx`) as a preview model and Voila! Animation is working!!



> place holder 15_

![Animation running!][def_15_animation_running]


Hope this guide helps you!
Thanks for reading!


[def]: /unity_umotion_export_pipeline_guide/1_character_fckd_up.png
[def2]: /unity_umotion_export_pipeline_guide/2_character_corrected.png
[def_before_deletion]: /unity_umotion_export_pipeline_guide/3_before_elimination.png
[def_4_after_deletion]: /unity_umotion_export_pipeline_guide/4_after_elimination.png
[def_5_export_options]: /unity_umotion_export_pipeline_guide/5_export_options.png
[def_6_pose_editor_options]: /unity_umotion_export_pipeline_guide/6_pose_editor_setup.png
[def_7_clip_editor_first_step]: /unity_umotion_export_pipeline_guide/7_clipeditor_first_step.png
[def_8_clip_editor_window]: /unity_umotion_export_pipeline_guide/8_clip_import_window.png
[def_9_clip_editor_success]: /unity_umotion_export_pipeline_guide/9_clip_import_success.png
[def_10_character_animated_umotion]: /unity_umotion_export_pipeline_guide/10_character_being_animated.png
[def_11_no_export_dest_message]: /unity_umotion_export_pipeline_guide/11_no_export_destination.png
[def_12_clip_export_settings]: /unity_umotion_export_pipeline_guide/12_clip_settings.png
[def_13_clip_export_success]: /unity_umotion_export_pipeline_guide/13_export_success.png
[def_14_material_correction]: /unity_umotion_export_pipeline_guide/14_material_correction.png
[def_15_animation_running]: /unity_umotion_export_pipeline_guide/15_character_running.png