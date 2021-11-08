# @testing-library/react

[Counter Example](./counter_example.md)

---

[examples](https://github.com/harblaith7/React-Testing-Library-Net-Ninja)

Accessible by Everyone

- getByRole
- getByLabelText
- getByPlaceholderText
- getByText

Semantic Queries

- getByAltText
- getByTitle

Test ID

- getByTestId

|    Name    | No Match | 1 Match | 1+ Match | Await |
| :--------: | :------: | :-----: | :------: | :---: |
|   getBy    |  error   | return  |  error   |  no   |
|   findBy   |  error   | return  |  error   |  yes  |
|  queryBy   |   null   | return  |  error   |  no   |
|  getAllBy  |  error   |  array  |  array   |  no   |
| findAllBy  |  error   |  array  |  array   |  yes  |
| queryAllBy |  array   |  array  |  array   |  no   |
