# companion-module-propresenter6

Module for using ProPresenter with the Elgato Stream Deck and Companion. Requires a connection to the ProPresenter Remote network port.  Optionally, you can also configure a connection to the Stage Display App port if you want to track the Video CountDown Timer for any video that is playing.

# Commands
## Slides
Command | Description
------- | -----------
Next&nbsp;Slide | Advances to the next slide in the current document. If at the end of a document, will advance to the start of the next document in the playlist.
Previous&nbsp;Slide | Moves to the previous slide in the current document. If at the start of a document, will move to the start of the previous document in the playlist.
Specific&nbsp;Slide | Moves to that presentation/slide number. See the `Specific Slide` section below.

### Specific Slide
This action has two parameters:

**Slide Number**: Moves to the slide number specified.

A whole number greater than 0 will move the presentation to that slide number.

A slide number of `0` will trigger the current slide, which can be used to bring back a slide that was cleared using `Clear All` or `Clear Slide`.

A relative number (prefixed with `+` or `-`) will move the presentation +/- that many slides. `+3` will jump ahead three slides, and `-2` will jump back two slides. Try to avoid using `-1` or `+1` relative numbers; use the `Next Slide` and `Previous Slide` actions instead, as they perform better. 


**Presentation Path**: Lets you trigger a slide in a different presentation, even from a different playlist.

*Important: Presentation path numbering starts at 0, meaning `0` will trigger a slide in the first presentation in the playlist.*

A single number, like `3`, will let you trigger a slide in the *fourth* presentation in the **current playlist**. 

A path like `1:3` will trigger presentation #4 in playlist #2.

Playlists in groups (or nested groups) are identified using periods. `1.1.0:2` means "The second playlist is a group. The second item in that group is another group. Select the first playlist in that group. Choose the third presentation in the playlist."

The below image may make this more clear:

![specific-slide](documentation/images/specific-slide.png)


## Audio Cues
Command | Description
------- | -----------
Audio&nbsp;Start&nbsp;Cue | Start a specific audio cue in an audio-bin playlist.  Uses the same numerical format to specify the path of the audio item (see Presentation Path explanation above)
Audio&nbsp;Play/Pause | Pause (or resume playing) the currently playing (or paused) audio.


## Clear/Logo
Command | Description
------- | -----------
Clear&nbsp;All | Clears all the layers
Clear&nbsp;Audio | Clears the audio track
Clear&nbsp;Background | Clears only the background layer
Clear&nbsp;Slide | Clears the current slide (foreground and background)
Clear&nbsp;Telestrator | Clears all annotations drawn with the telestrator
Clear&nbsp;to&nbsp;Logo | Clears all the layers and shows the logo image set in ProPresenter

### Clear All
Note: When the `Clear All` action is triggered against ProPresenter for Windows, the current slide will be lost but on Mac it's preserved.

For example, if you're on slide #5, trigger `Clear All`, and then trigger `Next Slide`:
- On Mac you'll be on slide #6
- On Windows, you'll be on slide #1

You can work around this PC limitation by using the `Specific Slide` action with a relative slide number of `+1` to move to the next slide. This would move you to slide #6 after the `Clear All` action.

## Messages (On Output screen)
Command | Description
------- | -----------
Show&nbsp;Message | Shows the message on the stage display output. You can pass values for message tokens, but you must do so very carefully - a typo here can crash ProPresenter.  Crashes can cause data loss, and ruin your setup. Learn how to correctly enter message tokens by reading below. Always type carefully and double-check. Get it right on a test machine first! The correct way to pass values for message tokens is as two lists. The two lists work together to form token NAME and token VALUE pairs.  The first list is a comma-separated list of token NAMES and the second is a comma-separated list of token VALUES. The number of items in eash list should match - e.g. if you supply two token NAMES, then you should supply two token VALUES to make matching pairs. All token names in your list *MUST* match the token names defined in the message within ProPresenter (or else Pro6 will likely crash).  The token values can be any text. You don't have to pass *all* the token names/values pairs - any name/values that you don't include will be treated as and displayed as blank. You don't have to pass any token names/values if you don't need to. Static messages without any tokens are safe - you can't make a typo if you leave the token names and token values list blank! If one of your token names or token values needs to have a comma in it, you can type a double comma (,,) to insert a literal comma - this works in either a token name or a token value. Again, make certain that your list of token NAMES perfectly match the names of the tokens defined in the message within Pro6 - Pro6 won't crash if they match perfectly - so be carefull!
Hide&nbsp;Message | Removes a message from output screen.

Messages are identified by Index. Index is a 0-based, where the first message is 0, and then count up through the messages in the order shown in the list of ProPresenter messages.

## Stage Display
Command | Description
------- | -----------
Stage&nbsp;Display&nbsp;Message | Shows the message on the stage display output
Stage&nbsp;Display&nbsp;Hide&nbsp;Message | Removes the stage display message
Stage&nbsp;Display&nbsp;Layout | Sets the stage display layout. 

Stage Displays are indentified by index. Index is a 0-based number, where the first layout is 0 and then count up through the stage display layouts in the order shown in ProPresenters list of stage display layouts.


## Clocks (Timers)
Command | Description
------- | -----------
Start&nbsp;Clock | Starts clock (timer) - identified by index (0 based)
Stop&nbsp;Clock | Stops clock (timer) - identified by index (0 based)
Reset&nbsp;Clock | Resets clock (timer) - identified by index (0 based)
Update&nbsp;Clock | Update clock/timer with a new duration - identified by index (0 based). You must specify the type of clock as either Count Down Timer, Count Down To Time or Elapsed Time. (Note that any clock you update will be changed to the the selected type.)  "Duration" is the new duration value for the count-down timer in the format HH:MM:SS. (It is also the starting time for Elapsed Time clocks. You may also use a shorthand format if you like. You can, if you want, leave out the HH and/or the MM values and they will default to zero - you can also leave out one or both of the ":" to enter just mins and/or seconds.  You can control overrun for all clock types.  AM/PM is only needed for Count Down To Time clocks.

**Tip: One-Touch Preset CountDown Timers.**
If you use a lot of timers with commonly used values for duration, you might like to setup a few buttons that automatically reset and restart a count-down timer for your most commonly used durations. To make a single button do that for you, you can chain together the following three actions:
1. *Update CountDown Clock* - Set new duration value of the count-down timer. This new value will be used when the timer is next reset.
2. *Reset Clock* - Stop the count-down timer if running and reset current value back to duration. You  might like to add a little delay (say 100-300ms) to ensure ProPresenter has time to process previous action.
3. *Start Clock* - Start the count-down timer running. You might like to add a little delay (say 100-300ms) to ensure ProPresenter has time to process previous action.

Use *relative delays*, to ensure these three action arrive in the correct order (or if you prefer absolute delays, make sure the second action has less delay than the third)

# Dynamic Variables
Variable | Description
-------- | -----------
$(propresenter:current_slide) | The number of the active slide (>= 1), or "N/A" if unknown.
$(propresenter:total_slides)  | The total number of slides in the current document, or "N/A" if unknown.
$(propresenter:presentation_name) | The name of the current presentation, or "N/A" if unknown.
$(propresenter:connection_status) | The current connection status to ProPresenter ("Disconnected" or "Connected").
$(propresenter:watched_clock_current_time) | In the config of this module, you can specify the index of a clock (timer) that you want to "watch". This dynamic variable will be updated once per second to the current value of the clock specified. You could use this to display a live timer value on a button!
$(propresenter:current_stage_display_index) | Index of the currently selected stage display layout (This is updated whenever a new layout is selected.)
$(propresenter:current_stage_display_name) | Name of the currently selected stage display layout (This is updated whenever a new layout is selected.)
$(propresenter:video_countdown_timer) | Current value of video countdown timer - automatically updated when a video is playing. (This one variable is only updated when the module is configured to also connect to the Stage Display App port)
