# async/await

```py
def test1():
    print("a")
    print("b")
    print("c")

test1()

a
b
c
```

```py
import asyncio

async def test1():
    print("a")
    print("b")
    print("c")

asyncio.run(test1())

a
b
c
```