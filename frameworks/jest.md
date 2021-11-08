# matchers

`sum.js`

```jsx
function sum(a, b) {
  return a + b
}
module.exports = sum
```

`sum.test.js`

```jsx
const sum = require('./sum')

it('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3)
})

it('object assignment', () => {
  const data = { one: 1 }
  data['two'] = 2
  expect(data).toEqual({ one: 1, two: 2 })
})

it('adding positive numbers is not zero', () => {
  for (let a = 1; a < 10; a++) {
    for (let b = 1; b < 10; b++) {
      expect(a + b).not.toBe(0)
    }
  }
})

// TRUTHY AND FALSEY

it('null', () => {
  const n = null
  expect(n).toBeNull()
  expect(n).toBeDefined()
  expect(n).not.toBeUndefined()
  expect(n).not.toBeTruthy()
  expect(n).toBeFalsy()
})

it('zero', () => {
  const z = 0
  expect(z).not.toBeNull()
  expect(z).toBeDefined()
  expect(z).not.toBeUndefined()
  expect(z).not.toBeTruthy()
  expect(z).toBeFalsy()
})

// NUMBERS

it('two plus two', () => {
  const value = 2 + 2
  expect(value).toBeGreaterThan(3)
  expect(value).toBeGreaterThanOrEqual(3.5)
  expect(value).toBeLessThan(5)
  expect(value).toBeLessThanOrEqual(4.5)

  // toBe and toEqual are equivalent for numbers
  expect(value).toBe(4)
  expect(value).toEqual(4)
})

it('adding floating point numbers', () => {
  const value = 0.1 + 0.2
  //expect(value).toBe(0.3);           This won't work because of rounding error
  expect(value).toBeCloseTo(0.3) // This works.
})

// STRINGS //

it('there is no I in team', () => {
  expect('team').not.toMatch(/I/)
})

it('but there is a "stop" in Christoph', () => {
  expect('Christoph').toMatch(/stop/)
})

// ARRAYS //

const shoppingList = [
  'diapers',
  'kleenex',
  'trash bags',
  'paper towels',
  'milk',
]

it('the shopping list has milk on it', () => {
  expect(shoppingList).toContain('milk')
  expect(new Set(shoppingList)).toContain('milk')
})

// EXCEPTIONS //

function compileAndroidCode() {
  throw new Error('you are using the wrong JDK')
}

it('compiling android goes as expected', () => {
  expect(() => compileAndroidCode()).toThrow()
  expect(() => compileAndroidCode()).toThrow(Error)

  // You can also use the exact error message or a regexp
  expect(() => compileAndroidCode()).toThrow('you are using the wrong JDK')
  expect(() => compileAndroidCode()).toThrow(/JDK/)
})
```

# async

`async.js`

```jsx
const axios = require('axios')

const fetchData = async (id) => {
  const results = await axios.get(
    `https://jsonplaceholder.typicode.com/todos/${id}`
  )
  return results.data
}

module.exports = fetchData
```

`async.test.js`

```jsx
const fetchData = require('./async')

it('should return correct todo', () => {
  fetchData(1).then((todo) => {
    expect(todo.id).toBe(1)
  })
})

it('should return correct todo', async () => {
  const todo = await fetchData(1)
  expect(todo.id).toBe(1)
})
```

# setup

`setup.test.js`

```jsx
let animals = ['elephant', 'zebra', 'bear', 'tiger']

beforeEach(() => {
  animals = ['elephant', 'zebra', 'bear', 'tiger']
})

describe('animals array', () => {
  it('should add animal to end of array', () => {
    animals.push('aligator')
    expect(animals[animals.length - 1]).toBe('aligator')
  })

  it('should add animal to beginning of array', () => {
    animals.unshift('monkey')
    expect(animals[0]).toBe('monkey')
  })

  it('should have initial length of 4', () => {
    expect(animals.length).toBe(4)
  })
})
```

# mocks

`mock.test.js`

```jsx
const axios = require('axios')

const fetchData = async (id) => {
  console.log('called')
  const results = await axios.get(
    `https://jsonplaceholder.typicode.com/todos/${id}`
  )
  return results
}

function forEach(items, callback) {
  for (let index = 0; index < items.length; index++) {
    callback(items[index])
  }
}

it('mock callback', () => {
  const mockCallback = jest.fn((x) => 42 + x)

  forEach([0, 1], mockCallback)

  expect(mockCallback.mock.calls.length).toBe(2)

  expect(mockCallback.mock.calls[0][0]).toBe(0)

  expect(mockCallback.mock.calls[1][0]).toBe(1)

  expect(mockCallback.mock.results[0].value).toBe(42)
})

it('return mock', () => {
  const mock = jest.fn()

  mock.mockReturnValueOnce(true).mockReturnValueOnce(false)

  const results = mock()
  const results2 = mock()

  expect(results).toBe(true)
  expect(results2).toBe(false)
})

it('mock modules or custom functions', async () => {
  jest.spyOn(axios, 'get').mockReturnValueOnce({
    id: 1,
    todo: 'Do youtube',
  })

  const results = await fetchData(1)

  expect(results.todo).toBe('Do youtube')
})
```
