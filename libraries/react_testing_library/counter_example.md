# Counter Example

`./Counter.jsx`

```jsx
import { useState } from 'react'
import './Counter.css'

const Counter = () => {
  const [counterValue, setCounterValue] = useState(0)
  const [inuptValue, setInuptValue] = useState(1)

  const addToCounter = () => {
    setCounterValue(counterValue + inuptValue)
  }

  const subtractFromCounter = () => {
    setCounterValue(counterValue - inuptValue)
  }

  return (
    <div>
      <h1 data-testid='header'>My Counter</h1>
      <p
        data-testid='counter'
        id='counter-value'
        className={
          counterValue >= 100 ? 'green' : counterValue <= -100 ? 'red' : ''
        }
      >
        {counterValue}
      </p>
      <button data-testid='subtract-btn' onClick={subtractFromCounter}>
        -
      </button>
      <input
        type='number'
        value={inuptValue}
        data-testid='input'
        className='text-center'
        onChange={(e) => setInuptValue(parseInt(e.target.value))}
      />
      <button data-testid='add-btn' onClick={addToCounter}>
        +
      </button>
    </div>
  )
}

export default Counter
```

`./__test__/Counter.test.js`

```jsx
import React from 'react'
import Counter from '../Counter'
import { render, fireEvent } from '@testing-library/react'
import '@testing-library/jest-dom/extend-expect'

let getByTestId

beforeEach(() => {
  const component = render(<Counter />)
  getByTestId = component.getByTestId
})

test('header renders with correct text', () => {
  const headerEl = getByTestId('header')
  expect(headerEl.textContent).toBe('My Counter')
})

test('counter initially starts with text of 0', () => {
  const counterEl = getByTestId('counter')
  expect(counterEl.textContent).toBe('0')
})

test('input contains inittial value of 1', () => {
  const inputEl = getByTestId('input')
  expect(inputEl.value).toBe('1')
})

test('add button renders with +', () => {
  const addBtn = getByTestId('add-btn')
  expect(addBtn.textContent).toBe('+')
})

test('add button renders with -', () => {
  const subtractBtn = getByTestId('subtract-btn')
  expect(subtractBtn.textContent).toBe('-')
})

test('change value of input works correctly', () => {
  const inputEl = getByTestId('input')
  expect(inputEl.value).toBe('1')

  fireEvent.change(inputEl, {
    target: {
      value: '5',
    },
  })

  expect(inputEl.value).toBe('5')
})

test('click on + button adds 1 to counter', () => {
  const addBtn = getByTestId('add-btn')
  const counterEl = getByTestId('counter')

  expect(counterEl.textContent).toBe('0')

  fireEvent.click(addBtn)

  expect(counterEl.textContent).toBe('1')
})

test('click on - button subtracts 1 to counter', () => {
  const subtractBtn = getByTestId('subtract-btn')
  const counterEl = getByTestId('counter')

  expect(counterEl.textContent).toBe('0')

  fireEvent.click(subtractBtn)

  expect(counterEl.textContent).toBe('-1')
})

test('changing input value then clicking on add btn works correctly', () => {
  const addBtn = getByTestId('add-btn')
  const counterEl = getByTestId('counter')
  const inputEl = getByTestId('input')

  fireEvent.change(inputEl, {
    target: {
      value: '5',
    },
  })

  fireEvent.click(addBtn)

  expect(counterEl.textContent).toBe('5')
})

test('changing input value then clicking on subtract btn works correctly', () => {
  const subtractBtn = getByTestId('subtract-btn')
  const counterEl = getByTestId('counter')
  const inputEl = getByTestId('input')

  fireEvent.change(inputEl, {
    target: {
      value: '5',
    },
  })

  fireEvent.click(subtractBtn)

  expect(counterEl.textContent).toBe('-5')
})

test('adding and the subtracting leads to the correct counter number', () => {
  const addBtn = getByTestId('add-btn')
  const subtractBtn = getByTestId('subtract-btn')
  const counterEl = getByTestId('counter')
  const inputEl = getByTestId('input')

  fireEvent.change(inputEl, {
    target: {
      value: '10',
    },
  })

  fireEvent.click(addBtn)
  fireEvent.click(addBtn)
  fireEvent.click(addBtn)
  fireEvent.click(addBtn)
  fireEvent.click(subtractBtn)
  fireEvent.click(subtractBtn)

  expect(counterEl.textContent).toBe('20')

  fireEvent.change(inputEl, {
    target: {
      value: '5',
    },
  })

  fireEvent.click(addBtn)
  fireEvent.click(subtractBtn)
  fireEvent.click(subtractBtn)

  expect(counterEl.textContent).toBe('15')
})

test('counter countains correct className', () => {
  const counterEl = getByTestId('counter')
  const addBtn = getByTestId('add-btn')
  const subtractBtn = getByTestId('subtract-btn')
  const inputEl = getByTestId('input')

  expect(counterEl.className).toBe('')

  fireEvent.change(inputEl, {
    target: {
      value: '50',
    },
  })

  fireEvent.click(addBtn)

  expect(counterEl.className).toBe('')

  fireEvent.click(addBtn)

  expect(counterEl.className).toBe('green')

  fireEvent.click(addBtn)

  expect(counterEl.className).toBe('green')

  fireEvent.click(subtractBtn)
  fireEvent.click(subtractBtn)

  expect(counterEl.className).toBe('')

  fireEvent.click(subtractBtn)
  fireEvent.click(subtractBtn)
  fireEvent.click(subtractBtn)
  fireEvent.click(subtractBtn)

  expect(counterEl.className).toBe('red')
})
```
