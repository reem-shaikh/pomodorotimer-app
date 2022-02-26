# pomodorotimer-app
### Npm Packages 
1. react-circular-progressbar from `https://www.npmjs.com/`
A circular progressbar component, built with SVG and extensively customizable
> follow the documentation: 
- https://www.npmjs.com/package/@tomik23/react-circular-progress-bar 
- https://github.com/kevinsqi/react-circular-progressbar

#### Installation
```bash 
npm install --save react-circular-progressbar
```
#### Usage
Import the component and default styles:
```bash 
import { CircularProgressbar } from 'react-circular-progressbar';
import 'react-circular-progressbar/dist/styles.css';
```
> Note: If you don't have a CSS loader, you can copy styles.css into your project instead.

Now you can use the component:
```bash 
const percentage = 66;

<CircularProgressbar value={percentage} text={`${percentage}%`} />;
```
2. react-slider 
> follow the documentation:
- https://www.npmjs.com/package/react-slider

##### Installation
```bash 
npm install react-slider
```
#### Usage 
```bash 
import react-slider
```
### Icons 
2. Heroicons `https://heroicons.com/` (import JSX code)
- play icon 
- pause icon 
- settings icon 
- back button icon 

#### The Logic 
1. we created a context object in settingsContext.js 
```bash 
import react from 'react'; 
const SettingsContext = react.createContext({});

export default SettingsContext;
```
2. The provider property is used in App.js is used to provide some data to the child components 
> App.js 
```bash 
return (
    <main>
      <SettingsContext.Provider 
      ✅value={{
        showSettings,
        setShowSettings,
        workMinutes,
        breakMinutes,
        // setting new values for workmins and breakmins 
        setWorkMinutes,
        setBreakMinutes,
      }}>
        {/* if showSettings is true, then show settings component */}
        {/* when someone clicks on the settings button we toggle it to true using setShowSettings state */}

        {showSettings ? <Settings /> : <Timer />}
      </SettingsContext.Provider>
    </main>
  );
```
3. We have created 3 states using useState hook and were passing it through the provider property of context object 
> App.js 
```bash 
import './App.css';
import Timer from "./Timer";
import Settings from "./Settings";
import {useState} from "react";
import SettingsContext from "./SettingsContext";

function App() {

  ✅const [showSettings, setShowSettings] = useState(false);

# these 2 states are to set the workminutes and breakminutes 
  ✅const [workMinutes, setWorkMinutes] = useState(45);
  ✅const [breakMinutes, setBreakMinutes] = useState(15);

  return (
    <main>
      <SettingsContext.Provider value={{
        showSettings,
        setShowSettings,
        workMinutes,
        breakMinutes,
        // setting new values for workmins and breakmins 
        setWorkMinutes,
        setBreakMinutes,
      }}>
        {/* if showSettings is true, then show settings component */}
        {/* when someone clicks on the settings button we toggle it to true using setShowSettings state */}

        {showSettings ? <Settings /> : <Timer />}
      </SettingsContext.Provider>
    </main>
  );
}

export default App;
```
4. The values passed through provider are consumed in this component Settings.js
> Settings.js 
```bash 
import ReactSlider from 'react-slider';
import './slider.css'
import SettingsContext from "./SettingsContext";
import {useContext} from "react";
import BackButton from "./BackButton";


function Settings() {
# importing the settings context defined in app.js
  ✅const settingsInfo = useContext(SettingsContext);

  return(
    ✅We created 2 sliders over here using the react-slider library import 

    <div style={{textAlign:'left'}}>
    ✅the user can set the work minutes manually
      <label>work: {settingsInfo.workMinutes}:00</label>
      <ReactSlider
        className={'slider'}
        ✅thumb is the scrolling bar in the sllider 
        thumbClassName={'thumb'}
        trackClassName={'track'}

        value={settingsInfo.workMinutes}
        ✅when the thumb is scrolled onchange is triggered and set to the new value set by the user 
        onChange={newValue => settingsInfo.setWorkMinutes(newValue)}
        # can set work to 120 mins 
        min={1}
        max={120}
      />
      <label>break: {settingsInfo.breakMinutes}:00</label>
      <ReactSlider
        className={'slider green'}
        thumbClassName={'thumb'}
        trackClassName={'track'}
        value={settingsInfo.breakMinutes}
        onChange={newValue => settingsInfo.setBreakMinutes(newValue)}
        min={1}
        max={120}
      />

    # when you click on the backbutton setShowsettings is switched to false and according to the conditional rendering on App.js it shows the timer component   

      <div style={{textAlign:'center', marginTop:'20px'}}>
        <BackButton onClick={() => settingsInfo.setShowSettings(false)} />
        # when setshowsettings is toggled to false it removes the settings component from display and shows the settings component according to the logic we set at App.js 
      </div>

    </div>
  );
}

export default Settings;
```
5. Inside the timer component, were calling the button components we defined using the heroicons JSX import; which provides our app with the specific icon, all we goto do is pass it in a button tag and embed the ternary props inside it (which destructures all the props send to you from the parent (heroku icons) to the child component)

- BackButton.js 
```bash 
function BackButton(props) {
    return (
      <button {...props} className={'with-text'}>
        <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fillRule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm.707-10.293a1 1 0 00-1.414-1.414l-3 3a1 1 0 000 1.414l3 3a1 1 0 001.414-1.414L9.414 11H13a1 1 0 100-2H9.414l1.293-1.293z" clipRule="evenodd" />
        </svg>
        Back
      </button>
    );
  }
  
  export default BackButton;
```

- PlayButton.js 
```bash 
import React from 'react'
import './App.css'

function PlayButton(props) {
  return (
        <button {...props}>
    {/* import svg from https://heroicons.com/ */}
   <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
</svg>
        </button>
  )
}

export default PlayButton
```

- PauseButton.js 
```bash 
function PauseButton(props) {
    return (
      <button {...props}>
        <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fillRule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zM7 8a1 1 0 012 0v4a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v4a1 1 0 102 0V8a1 1 0 00-1-1z" clipRule="evenodd" />
        </svg>
      </button>
    );
  }
  
  export default PauseButton;
```

- SettingsButton.js 
```bash 
function SettingsButton(props) {
    return (
      <button {...props} className={'with-text'}>
        <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fillRule="evenodd" d="M11.49 3.17c-.38-1.56-2.6-1.56-2.98 0a1.532 1.532 0 01-2.286.948c-1.372-.836-2.942.734-2.106 2.106.54.886.061 2.042-.947 2.287-1.561.379-1.561 2.6 0 2.978a1.532 1.532 0 01.947 2.287c-.836 1.372.734 2.942 2.106 2.106a1.532 1.532 0 012.287.947c.379 1.561 2.6 1.561 2.978 0a1.533 1.533 0 012.287-.947c1.372.836 2.942-.734 2.106-2.106a1.533 1.533 0 01.947-2.287c1.561-.379 1.561-2.6 0-2.978a1.532 1.532 0 01-.947-2.287c.836-1.372-.734-2.942-2.106-2.106a1.532 1.532 0 01-2.287-.947zM10 13a3 3 0 100-6 3 3 0 000 6z" clipRule="evenodd" />
        </svg>
  
        Settings
      </button>
    );
  }
  
export default SettingsButton;
```
6. Inside the provider property of the context object, in App.js we've called timer component and settings component.
> App.js 
```bash 
    # if showSettings is true, then show settings component */}
    # when someone clicks on the settings button we toggle it to true using setShowSettings state */}
    {showSettings ? <Settings /> : <Timer />}
```
7. Inside Timer.js we've installed then imported circular-progress-bar from npm official site, for the circular ring of the pomodoro timer. 

> inside the return statement of Timer.js 
```bash 
// importing react-circular-progressbar 
import { CircularProgressbar, buildStyles } from 'react-circular-progressbar';
import 'react-circular-progressbar/dist/styles.css';

  return (
    <div>
    ✅were using the progressbar by using this tag 
    ✅in order to add styling weve imported the styles.css extensuon as well, to use it we use buildStyles 
      <CircularProgressbar
        value={percentage}
        # based on the percentage the value of the circular ring will be manuevered 
        text={minutes + ':' + seconds}
        # time is shown to us in the form of minutes : seconds 
        styles={buildStyles({
        textColor:'#fff',
        pathColor:mode === 'work' ? red : green,
        # pathColor changes the color of the circular ring based on which mode your in 
        tailColor:'rgba(255,255,255,.2)',
        # when the timer loads and leaves a trail it will be of this color 
      })} 
      />

      <div style={{marginTop:'20px'}}>
        {/* if its paused show play button after hiding the pause button; else show pause button */}
        # isPaused is a state used for togglig between pause and play buttons 
        {isPaused
          ? <PlayButton onClick={() => { setIsPaused(false); isPausedRef.current = false; }} />
          : <PauseButton onClick={() => { setIsPaused(true); isPausedRef.current = true; }} />}
      </div>

    # when you click on the settingsbutton its toggled to true, when its true then the settings component will show up 
      <div style={{marginTop:'20px'}}>
        <SettingsButton onClick={() => settingsInfo.setShowSettings(true)} />
      </div>
    </div>
  );
```
8. here's how the percentage, minutes and seconds logic will be calculated 
```bash 
# lets take a scenario where we click on the play button and pause it when the min:sec is at 44:56

# Note that: these 2 values changes based on the time we set in the work react-slider
# secondsLeft = 2696 
# totalSeconds = 2700

# the timer is set to 45 mins by default 
# 2700 / 60 = 45 
# 45mins x 60seconds 
# totalseconds = 2700seconds 

  console.log('seconds left', secondsLeft)
  console.log('total seconds', totalSeconds)
  const percentage = Math.round(secondsLeft / totalSeconds * 100);
# 2696 / 2700 * 100 = Math.round(99.85)
# 100%

  const minutes = Math.floor(secondsLeft / 60);
# 2696 / 60 = Math.floor(44.93) 
# minutes = 44
  
  let seconds = secondsLeft % 60;
# 2696 % 60 = 56 
# seconds = 56 

  if(seconds < 10) seconds = '0'+seconds;
# if seconds is one digit then add 0 string before it to make it look like a 2 digit number at all times 
```
9. Inside timer.js were using alot of hooks 
- useEffect - to load the changes as soon as the page is rendered 
- useContext - to recieve the props passed from provider context object 
- useRef - to ensure that the values are persisting between renders 
- useState - everytime the state is changed using setstate, the page is re-rendered and the previous value is lost, thats why were storing useState inside useRef hook 
```bash 
import {useContext, useState, useEffect, useRef} from "react";

# This is another way to consume the values given by the provider context object 
  const settingsInfo = useContext(SettingsContext);

  // using state to figure out whether its paused or not 
  // true implies its paused 
  // when application is loaded the counter is by default paused 
  const [isPaused, setIsPaused] = useState(true);

  const [mode, setMode] = useState('work'); 

  const [secondsLeft, setSecondsLeft] = useState(0);

  //useref hook allows you to persist the values between renders 
  //were storing usestate hooks inside useref hooks to persist these values 

  const secondsLeftRef = useRef(secondsLeft);
  const isPausedRef = useRef(isPaused);
  const modeRef = useRef(mode);
```
> how totalSeconds is calculated?
```bash 
  const totalSeconds = mode === 'work'
    ? settingsInfo.workMinutes * 60
    : settingsInfo.breakMinutes * 60;
```
> how secondLeft is calculated?
```bash 
  function tick() {
    //remove one sec everytime tick is called 
    secondsLeftRef.current--;
    setSecondsLeft(secondsLeftRef.current);
  }

  //useffect will be rendered as soon as app is loaded 
  useEffect(() => {
    function switchMode() {
    // setting the cycle mode 
    // work mode - red color 
    // break mode - green color 

      //if nextmode is work, then set it to break
      const nextMode = modeRef.current === 'work' ? 'break' : 'work';

      //if mode is work then show work mins else show break mins 
      const nextSeconds = (nextMode === 'work' ? settingsInfo.workMinutes : settingsInfo.breakMinutes) * 60; //secs

    # store the mode the user currently is in setmode state and log that value inside the useRef state to store it 
      setMode(nextMode);
      modeRef.current = nextMode;

    # store the second were currently at inside setsecondsleft state and update the value inside useRef state to store it 
      setSecondsLeft(nextSeconds);
      secondsLeftRef.current = nextSeconds;
    }

    //40mins x 60secs
    # based on the what second is currentlu logged into the useRef state secondsLeftRef it does the calculation, and sets the current second into the setsecondsLeft() 
    secondsLeftRef.current = settingsInfo.workMinutes * 60;
    setSecondsLeft(secondsLeftRef.current);

        const interval = setInterval(() => {
    # if its paused you dont do any ticking and pause the counter 
      if (isPausedRef.current) {
        return;
      }

    # otherwise if seconds is 0 then switch the mode 
    //from work to break or break to work 
      if (secondsLeftRef.current === 0) {
        return switchMode();
      }

      //if its not paused , then 
      //remove one sec at a time 
      tick();

    }, 1000);
    # call this function every 1000ms 

    return () => clearInterval(interval);
  }, [settingsInfo]);
# useEffect function ends 
```
> Timer.js (complete code)
```bash 
// importing react-circular-progressbar 
import { CircularProgressbar, buildStyles } from 'react-circular-progressbar';
import 'react-circular-progressbar/dist/styles.css';

import PlayButton from "./PlayButton";
import PauseButton from "./PauseButton";
import SettingsButton from "./SettingsButton";

import {useContext, useState, useEffect, useRef} from "react";
// importing states from app.js 
import SettingsContext from "./SettingsContext";

const red = '#f54e4e';
const green = '#4aec8c';

function Timer() {
  // importing settingscontext inside usecontext 
  const settingsInfo = useContext(SettingsContext);

  // using state to figure out whether its paused or not 
  // true implies its paused 
  // when application is loaded the counter is by default paused 
  const [isPaused, setIsPaused] = useState(true);
  const [mode, setMode] = useState('work'); // work/break/null
  const [secondsLeft, setSecondsLeft] = useState(0);

  //useref hook allows you to persist the values between renders 
  //were storing usestate hooks inside useref hooks to persist these values 
  const secondsLeftRef = useRef(secondsLeft);
  const isPausedRef = useRef(isPaused);
  const modeRef = useRef(mode);

  function tick() {
    //remove one sec everytime tick is called 
    secondsLeftRef.current--;
    setSecondsLeft(secondsLeftRef.current);
  }

  //useffect will be rendered as soon as app is loaded 
  useEffect(() => {
    function switchMode() {
    // setting the cycle mode 
    // work mode - red color 
    // break mode - green color 

      //if nextmode is work, then set it to break
      const nextMode = modeRef.current === 'work' ? 'break' : 'work';

      //if mode is work then show work mins else show break mins 
      const nextSeconds = (nextMode === 'work' ? settingsInfo.workMinutes : settingsInfo.breakMinutes) * 60;

      //store the mode the user currently is in 
      setMode(nextMode);
      modeRef.current = nextMode;

      setSecondsLeft(nextSeconds);
      secondsLeftRef.current = nextSeconds;
    }

    //45mins x 60secs
    secondsLeftRef.current = settingsInfo.workMinutes * 60;
    setSecondsLeft(secondsLeftRef.current);

    const interval = setInterval(() => {
    //if its paused you dont do any ticking and pause the counter 
      if (isPausedRef.current) {
        return;
      }
    //otherwise if seconds is 0 then switch the mode 
    //from work to break or break to work 
      if (secondsLeftRef.current === 0) {
        return switchMode();
      }

      //if its not paused , then 
      //remove one sec at a time 
      tick();

    }, 1000);

    return () => clearInterval(interval);
  }, [settingsInfo]);

  const totalSeconds = mode === 'work'
    ? settingsInfo.workMinutes * 60
    : settingsInfo.breakMinutes * 60;

  
  console.log('seconds left', secondsLeft)
  console.log('total seconds', totalSeconds)
  const percentage = Math.round(secondsLeft / totalSeconds * 100);
  console.log('percemtage', percentage)

  const minutes = Math.floor(secondsLeft / 60);
  console.log('minutes', minutes)
  
  let seconds = secondsLeft % 60;
  console.log('seconds', seconds)

  if(seconds < 10) seconds = '0'+seconds;

  return (
    <div>
      <CircularProgressbar
        value={percentage}
        text={minutes + ':' + seconds}
        styles={buildStyles({
        textColor:'#fff',
        pathColor:mode === 'work' ? red : green,
        tailColor:'rgba(255,255,255,.2)',
      })} 
      />

      <div style={{marginTop:'20px'}}>
        {/* if its paused show play button after hiding the pause button; else show pause button */}
        {isPaused
          ? <PlayButton onClick={() => { setIsPaused(false); isPausedRef.current = false; }} />
          : <PauseButton onClick={() => { setIsPaused(true); isPausedRef.current = true; }} />}
      </div>
      <div style={{marginTop:'20px'}}>
        <SettingsButton onClick={() => settingsInfo.setShowSettings(true)} />
      </div>
    </div>
  );
}

export default Timer;
```
