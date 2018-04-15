let y = 0
let x = 0
// Use variable "move" to decide whether the 
// mini bot should move or stop
let move = 0
// Use a buffer value to reduce sensitivity 
// to tilt
let buffer = 200

// Set radio group so that the micro:bits can
// communicate with each other
radio.setGroup(1)
basic.showString("T")

basic.forever(() => {
    // Press "A+B" to calibrate
    if (input.buttonIsPressed(Button.AB)) {
        x = input.acceleration(Dimension.X)
        y = input.acceleration(Dimension.Y)
        basic.showString("C")
    // Press "A" to start moving the mini bot
    } else if (input.buttonIsPressed(Button.A)) {
        move = 1
        basic.showString("M")
    }
    // Press "B" to stop the mini bot
    if (input.buttonIsPressed(Button.B)) {
        move = 0
        radio.sendString("S")
        basic.showString("S")
    }
    // If move has been enabled
    if (move == 1) {
        // Check if the y-value has changed to decide whether
        // to move forward or backward
        if (input.acceleration(Dimension.Y) < y - buffer) {
            // Check x-value to decide whether to move right, left
            // or straight
            if (input.acceleration(Dimension.X) > x + buffer) {
                // Send signal to signify moving in Forward Right
                // direction
                radio.sendString("FR")
                basic.showArrow(ArrowNames.NorthEast)
            } else if (input.acceleration(Dimension.X) < x - buffer) {
                // Send signal to signify moving in Forward Left
                // direction
                radio.sendString("FL")
                basic.showArrow(ArrowNames.NorthWest)
            } else {
                // Send signal to signify moving in Forward direction
                radio.sendString("F")
                basic.showArrow(ArrowNames.North)
            }
        } else if (input.acceleration(Dimension.Y) > y + buffer) {
            if (input.acceleration(Dimension.X) > x + buffer) {
                // Send signal to signify moving in Backward Right
                // direction
                radio.sendString("BR")
                basic.showArrow(ArrowNames.SouthEast)
            } else if (input.acceleration(Dimension.X) < x - buffer) {
                // Send signal to signify moving in Backward Left
                // direction
                radio.sendString("BL")
                basic.showArrow(ArrowNames.SouthWest)
            } else {
                // Send signal to signify moving in Backward direction
                radio.sendString("B")
                basic.showArrow(ArrowNames.South)
            }
        }
    } else {
        // Send signal to stop the bot
        radio.sendString("S")
        basic.showString("S")
    }
})
