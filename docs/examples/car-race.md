# car-race

```typescript
game.splash("Car Race", "Avoid the borders!")

const car = sprites.create(img`
 . . 9 9 
 . 4 9 9 4
 . 4 9 9 4
 . . 9 9 
 . 9 9 9 9 
 4 9 9 9 9 4
 4 9 9 9 9 4
 . 9 9 9 9 
 9 9 9 9 9 9
 `)
car.y = 110


const roadImg = screen.clone()
let curr = 64
let target = curr
let roadW = 60
let numCol = 0
let isRed = false
let ddx = 0


function drive(lines: number) {
    roadImg.scroll(0, lines)
    while (--lines >= 0) {
        let w2 = roadW >> 1
        if (curr == target) {
            target = Math.randomRange(w2, 128 - w2)
        }
        const dx = Math.sign(target - curr) * ddx
        curr += dx
        roadImg.fillRect(0, lines, 128, 1, 7)
        roadImg.fillRect(curr - w2 - 5, lines, roadW + 10, 1, isRed ? 10 : 15)
        roadImg.fillRect(curr - w2, lines, roadW, 1, 0)

        if (numCol++ > 10) {
            numCol = 0
            isRed = !isRed
        }
    }
}

drive(128)
ddx = 1
const road = sprites.create(roadImg)
road.type = 1

car.onOverlap(function (other: Sprite) {
    game.over()
})
car.z = 10

let pos = 0
let prevPos = 0


game.update(function () {
    pos += 1 + Math.sqrt(control.millis()) / 200.0
    drive(pos - prevPos)
    prevPos = pos | 0
    info.setScore(prevPos >> 6)
    car.x += keys.dx(70)
})
```
