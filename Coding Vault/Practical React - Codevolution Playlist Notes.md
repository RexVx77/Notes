## Overview
This playlist covers practical NPM packages and tools for React applications, providing quick tutorials on how to install, configure, and use them to handle common real-world requirements (like icons, modals, and tooltips) without reinventing the wheel.

## 1. Introduction
* **Goal:** Introduce useful NPM packages for React applications that come in handy for building robust side projects and enterprise apps.

## 2. Icons (`react-icons`)
* **Library:** `react-icons`
* **Installation:** `yarn add react-icons`
* **Usage:** Import specific icons from their respective packs (e.g., FontAwesome `fa`, Material Design `md`). You can use inline props like `color` and `size`. To apply consistent styling across your entire app, wrap your top-level component in an `IconContext.Provider`.

```jsx
import { FaReact } from 'react-icons/fa';
import { MdAlarm } from 'react-icons/md';
import { IconContext } from 'react-icons';

// Usage with IconContext for global styling
<IconContext.Provider value={{ color: 'blue', size: '5rem' }}>
    <div className="App">
        {/* Inherits blue and 5rem */}
        <FaReact /> 
        {/* Overrides context with inline props */}
        <MdAlarm color="purple" size="10rem" /> 
    </div>
</IconContext.Provider>
```

## 3. Toast Notifications (`react-toastify`)
* **Library:** `react-toastify`
* **Installation:** `yarn add react-toastify`
* **Usage:** Import the `toast` object and the provided CSS file. Call `toast.configure()` at the root level. You can trigger toasts using methods like `toast()`, `toast.success()`, `toast.warn()`, etc.
* **Configurations:** Pass a configuration object to set the `position` (e.g., `toast.POSITION.TOP_LEFT`) and `autoClose` duration in milliseconds (or `false` to disable auto-close).

```jsx
import { toast } from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';

// Initialize at the root of your app
toast.configure();

// Triggering a custom toast notification
const notify = () => {
    toast.success('Operation successful!', {
        position: toast.POSITION.TOP_RIGHT,
        autoClose: 5000 // Closes after 5 seconds
    });
};

<button onClick={notify}>Show Toast</button>
```

## 4. Modal (`react-modal`)
* **Library:** `react-modal`
* **Installation:** `yarn add react-modal`
* **Usage:** Use a boolean state variable to control visibility via the `isOpen` prop. Handle closing via the `onRequestClose` prop (fires on overlay clicks or the Esc key). Use `Modal.setAppElement('#root')` to avoid accessibility warnings.

```jsx
import { useState } from 'react';
import Modal from 'react-modal';

// Bind to root element for screen readers
Modal.setAppElement('#root');

const ModalDemo = () => {
    const [modalIsOpen, setModalIsOpen] = useState(false);

    return (
        <div>
            <button onClick={() => setModalIsOpen(true)}>Open Modal</button>
            <Modal 
                isOpen={modalIsOpen} 
                onRequestClose={() => setModalIsOpen(false)} 
                shouldCloseOnOverlayClick={false}
                style={{
                    overlay: { backgroundColor: 'rgba(0,0,0,0.5)' },
                    content: { color: 'orange' }
                }}
            >
                <h2>Modal Title</h2>
                <button onClick={() => setModalIsOpen(false)}>Close</button>
            </Modal>
        </div>
    );
};
```

## 5. Tooltip (`@tippyjs/react`)
* **Library:** `@tippyjs/react`
* **Installation:** `yarn add @tippyjs/react`
* **Usage:** Wrap the target element with the `<Tippy>` component. The `content` prop can be a string, an HTML element, or a custom React component. You can configure properties like `arrow`, `delay`, and `placement`.
* **Note:** If the child of the `<Tippy>` component is a custom React component, you *must* wrap that child component in `forwardRef` so Tippy can access the underlying DOM node.

```jsx
import Tippy from '@tippyjs/react';
import 'tippy.js/dist/tippy.css';

const TooltipDemo = () => {
    return (
        <Tippy 
            content={<span style={{ color: 'yellow' }}>Colored Tooltip</span>} 
            arrow={false} 
            delay={1000} 
            placement="right"
        >
            <button>Hover me</button>
        </Tippy>
    );
};
```

## 6. CountUp (`react-countup`)
* **Library:** `react-countup`
* **Installation:** `yarn add react-countup`
* **Usage:** Animates numbers starting from a base value to a target value. Great for dashboards. You can use props like `start`, `end`, `duration`, `prefix`, and `decimals`.
* **Hook Implementation:** The `useCountUp` hook provides methods for manual, event-driven control of the animation (`start`, `pauseResume`, `reset`, `update`).

```jsx
import CountUp, { useCountUp } from 'react-countup';

const CountUpDemo = () => {
    // Hook implementation for manual control
    const { countUp, start, reset, update } = useCountUp({ 
        duration: 5, 
        end: 10000,
        startOnMount: false 
    });

    return (
        <div>
            {/* Basic Component Usage */}
            <CountUp start={0} end={1000} duration={5} prefix="$" decimals={2} />
            
            {/* Hook Usage */}
            <h1>{countUp}</h1>
            <button onClick={start}>Start</button>
            <button onClick={reset}>Reset</button>
            <button onClick={() => update(2000)}>Update to 2000</button>
        </div>
    );
};
```

## 7. Idle Timer (`react-idle-timer`)
* **Library:** `react-idle-timer`
* **Installation:** `yarn add react-idle-timer`
* **Usage:** Detects user inactivity by watching mouse, keyboard, and scroll events. Define a reference using `useRef` and attach it to the `<IdleTimer>` component.
* **Session Timeout Pattern:** On idle, trigger a modal warning the user. Start a `setTimeout` for a final countdown. If the user clicks "Stay Active", clear the timeout. If not, the timeout executes the logout function.

```jsx
import { useRef } from 'react';
import IdleTimer from 'react-idle-timer';

const IdleTimerDemo = () => {
    const idleTimerRef = useRef(null);

    const onIdle = () => {
        console.log('User has been idle for 5 seconds!');
        // Logic to show a session timeout modal goes here
    };

    return (
        <div>
            <IdleTimer 
                ref={idleTimerRef} 
                timeout={5000} 
                onIdle={onIdle} 
            />
            <h2>Move your mouse or wait 5 seconds...</h2>
        </div>
    );
};
```

## 8. Color Picker (`react-color`)
* **Library:** `react-color`
* **Installation:** `yarn add react-color`
* **Usage:** Provides various styled color picker components (e.g., `ChromePicker`). Maintain the selected color in a state variable. Pass the state to the `color` prop and update it via the `onChange` prop (extracting `updatedColor.hex`). It is commonly paired with a button to conditionally toggle visibility.

```jsx
import { useState } from 'react';
import { ChromePicker } from 'react-color';

const ColorPickerDemo = () => {
    const [color, setColor] = useState('#fff');
    const [showPicker, setShowPicker] = useState(false);

    return (
        <div>
            <button onClick={() => setShowPicker(!showPicker)}>
                {showPicker ? 'Close Picker' : 'Pick a Color'}
            </button>
            
            {showPicker && (
                <ChromePicker 
                    color={color} 
                    onChange={updatedColor => setColor(updatedColor.hex)} 
                />
            )}
            <h2>Selected Color: {color}</h2>
        </div>
    );
};
```

## 9. Credit Cards (`react-credit-cards`)
* **Library:** `react-credit-cards`
* **Installation:** `yarn add react-credit-cards`
* **Usage:** Renders an animated, visual credit card UI based on user input. Create a form to capture `number`, `name`, `expiry`, and `cvc` in state. 
* **Flipping Animation:** Track input focus to pass to the `focused` prop. Passing `e.target.name` to this prop automatically flips the UI card to show the back when the CVC field is focused.

```jsx
import { useState } from 'react';
import Cards from 'react-credit-cards';
import 'react-credit-cards/es/styles-compiled.css';

const CreditCardDemo = () => {
    const [number, setNumber] = useState('');
    const [name, setName] = useState('');
    const [expiry, setExpiry] = useState('');
    const [cvc, setCvc] = useState('');
    const [focus, setFocus] = useState('');

    return (
        <div>
            <Cards 
                number={number} 
                name={name} 
                expiry={expiry} 
                cvc={cvc} 
                focused={focus} 
            />
            <form>
                <input 
                    type="tel" 
                    name="number" 
                    placeholder="Card Number"
                    value={number} 
                    onChange={e => setNumber(e.target.value)} 
                    onFocus={e => setFocus(e.target.name)} 
                />
                <input 
                    type="tel" 
                    name="cvc" 
                    placeholder="CVC"
                    value={cvc} 
                    onChange={e => setCvc(e.target.value)} 
                    onFocus={e => setFocus(e.target.name)} 
                />
            </form>
        </div>
    );
};
```

## 10. Date Picker (`react-datepicker`)
* **Library:** `react-datepicker`
* **Installation:** `yarn add react-datepicker`
* **Usage:** Bind a date state variable using the `selected` prop and update it via `onChange`.
* **Helpful Props:** Use `dateFormat` to customize the string format. Use `minDate` or `maxDate` to restrict selectable ranges. Use `filterDate` to disable specific days (like weekends). Enable quick navigation with `showYearDropdown`.

```jsx
import { useState } from 'react';
import DatePicker from 'react-datepicker';
import 'react-datepicker/dist/react-datepicker.css';

const DatePickerDemo = () => {
    const [selectedDate, setSelectedDate] = useState(null);

    return (
        <DatePicker 
            selected={selectedDate} 
            onChange={date => setSelectedDate(date)} 
            dateFormat="dd/MM/yyyy"
            minDate={new Date()} // Disables past dates
            filterDate={date => date.getDay() !== 6 && date.getDay() !== 0} // Disables weekends
            isClearable
            showYearDropdown
            scrollableYearDropdown
        />
    );
};
```

## 11. Presentation Deck (`mdx-deck`)
* **Library:** `mdx-deck`
* **Installation:** `yarn add -D mdx-deck`
* **Usage:** Create `.mdx` files mixing standard Markdown with JSX. Use `---` to separate individual slides. You can embed live, interactive React components directly within your presentation slides and apply global themes.

```mdx
import { Counter } from '../components/Counter.js';
import { themes } from 'mdx-deck';

export const theme = themes.future;

<Header>Codevolution</Header>
<Footer>Practical React</Footer>

# Slide 1
Hello World! This is an MDX Deck presentation.

---

# Slide 2: Interactive Component
Here is a live React component inside the slide:
<Counter />

---

# Slide 3: Step reveal
<Steps>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</Steps>
```

## 12. Video Player (`react-player`)
* **Library:** `react-player`
* **Installation:** `yarn add react-player`
* **Usage:** A highly versatile player that supports multiple platforms (YouTube, Twitch, Vimeo). Pass the media URL to the `url` prop. Use `controls={true}` to display the browser's native player controls. You can listen to playback events using callback props.

```jsx
import ReactPlayer from 'react-player';

const VideoPlayerDemo = () => {
    return (
        <ReactPlayer 
            url="[https://www.youtube.com/watch?v=LZhwNGpiTEI](https://www.youtube.com/watch?v=LZhwNGpiTEI)" 
            controls={true}
            width="480px" 
            height="240px"
            onReady={() => console.log('Video is ready')}
            onStart={() => console.log('Video started')}
            onPause={() => console.log('Video paused')}
            onEnded={() => console.log('Video ended')}
        />
    );
};
```

## 13. Loading Indicators (`react-spinners`)
* **Library:** `react-spinners`
* **Installation:** `yarn add react-spinners @emotion/core`
* **Usage:** Offers various animated spinner components. Visibility is controlled via the `loading` boolean prop. Highly customizable using `size` and `color` props. You can use `@emotion/core` to pass CSS directly to the component.

```jsx
import { BounceLoader } from 'react-spinners';
import { css } from '@emotion/core';

const loaderCSS = css`
    margin-top: 25px;
    margin-bottom: 25px;
`;

const SpinnerDemo = () => {
    return (
        <BounceLoader 
            loading={true} 
            size={48} 
            color="red" 
            css={loaderCSS} 
        />
    );
};
```

## 14. Charts (`react-chartjs-2`)
* **Library:** `react-chartjs-2`, `chart.js`
* **Installation:** `yarn add react-chartjs-2 chart.js`
* **Usage:** Render `Line`, `Bar`, and `Doughnut` charts by passing a `data` object and an `options` object. 
* **Configuration:** Provide `labels` for the x-axis and an array of `datasets`. For Bar and Doughnut charts, `backgroundColor` should be an array of different colors corresponding to each data point.

```jsx
import { Line } from 'react-chartjs-2';

const ChartDemo = () => {
    const data = {
        labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May'],
        datasets: [
            {
                label: 'Sales 2020 (M)',
                data: [3, 2, 2, 1, 5],
                borderColor: ['rgba(255, 206, 86, 0.2)'],
                backgroundColor: ['rgba(255, 206, 86, 0.2)'],
                pointBackgroundColor: 'rgba(255, 206, 86, 0.2)' 
            }
        ]
    };

    const options = {
        title: { display: true, text: 'Line Chart' },
        scales: {
            yAxes: [{ ticks: { min: 0, max: 6, stepSize: 1 } }]
        }
    };

    return <Line data={data} options={options} />;
};
```

## 15. Debugging React Apps (React Developer Tools)
* **Tool:** React Developer Tools (Browser Extension for Chrome/Firefox/Edge)
* **Components Tab:** * Inspect your component tree and view/mutate props, state, and hooks on the fly to test UI states without refreshing the page.
  * Use the "bug" icon to log component details to the console, the `<>` icon to jump directly to the source code, and the warning icon to force an error to test your App's Error Boundaries.
* **Profiler Tab:** * Record performance to identify slow renders. 
  * Use the **Flame chart** and **Ranked chart** views to find performance bottlenecks (yellow bars indicate slow renders). It's crucial for verifying if `useMemo` or `useCallback` optimizations successfully reduced render times.
* **Settings:**
  * *Highlight updates when components render:* Draws flashing blue boxes around components as they render. Essential for spotting unnecessary re-renders.
  * *Hide logs during additional invocations in strict mode:* Cleans up your console by hiding the duplicate logs React fires in development mode.
  * *Record why each component rendered:* Enabling this in the Profiler settings tells you exactly what triggered a render (e.g., "Hook 1 changed" or "Parent component rendered").