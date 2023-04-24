```
#include<GL/glut.h>
#include<iostream>//for strlen
#include<stdlib.h>


int i, q;
int score = 0;//for score counting
int screen = 0;
bool collide = false;//check if car collide to make game over
char buffer[10];

int vehicleX = 200, vehicleY = 70;
int ovehicleX[4], ovehicleY[4];
int divx = 250, divy = 4, movd;

```



## Libraries:
--------------------------
- **<GL/glut.h>**: 
  - This is the OpenGL Utility Toolkit library which provides a set of tools to create graphical user interfaces in OpenGL.
  - In this program, it is used for creating windows, handling input, and drawing basic shapes.

- **iostream** This is the input/output stream library in C++. In this program, it is used for outputting the score on the screen.

- **<stdlib.h>** : This is the standard library in C++. In this program, it is used for generating ***random numbers.***
Here's what each variable does:

1) i: A loop variable used in various for loops.  
2) q: A loop variable used in various for loops.  
3) score: An integer variable that keeps track of the score of the player.
4) screen: An integer variable that determines the current state of the game screen. It is used to display different screens for different states (e.g. game menu, game over screen).  
5) collide: A boolean variable that is used to check if the car has collided with any other vehicle. If true, it triggers the game over condition.
6) buffer: A character array used to store the score as a string.
7) vehicleX: An integer variable that represents the x-coordinate of the player's car.
8) vehicleY: An integer variable that represents the y-coordinate of the player's car.
9) ovehicleX: An array of 4 integer variables that represents the x-coordinates of the other vehicles.
10) ovehicleY: An array of 4 integer variables that represents the y-coordinates of the other vehicles.
11) divx: An integer variable that represents the width of the road divider.
12) divy: An integer variable that represents the height of the road divider.
12) movd: An integer variable that represents the speed of the game. It is used to move the other vehicles and the road divider.


-------------------------
-------------------------
```
void drawText(char ch[], int xpos, int ypos)//draw the text for score and game over
{
    int numofchar = strlen(ch);
    glLoadIdentity();
    glRasterPos2f(xpos, ypos);
    for (i = 0; i <= numofchar - 1; i++)
    {

        glutBitmapCharacter(GLUT_BITMAP_HELVETICA_18, ch[i]);//font used here, may use other font also
    }
}
```

## Draw the text for score and game over:

- This is a function written in C/C++ that uses OpenGL to draw text on the screen. The function takes in a __character array (string), an x position, and a y position as parameters.__ It first calculates the number of characters in the string using the __strlen()__ function. It then _sets the current matrix to the identity matrix_ using __glLoadIdentity()__, which is used to clear any previous transformations that might have been applied.

- Next, it sets the _raster position_ for the text using __glRasterPos2f().__ This function takes in two parameters, the __x and y coordinates__ of the position to draw the text. The __glutBitmapCharacter()__ function is then used _to draw each character in the string, one at a time._ It takes in two parameters: __the font__ to use (in this case GLUT_BITMAP_HELVETICA_18, a built-in font in OpenGL), and __the character__ to draw.

- Overall, this function can be used to draw text on the screen in an OpenGL-based application, such as a game or simulation. The text can be positioned at any x and y coordinates on the screen, and different fonts can be used by changing the GLUT_BITMAP_HELVETICA_18 parameter to another built-in font or a custom font.

---------
--------- 

```
void drawTextNum(char ch[], int numtext, int xpos, int ypos)//counting the score
{
    int len;
    int k;
    k = 0;
    len = numtext - strlen(ch);
    glLoadIdentity();
    glRasterPos2f(xpos, ypos);
    for (i = 0; i <= numtext - 1; i++)
    {
        if (i < len)
            glutBitmapCharacter(GLUT_BITMAP_HELVETICA_18, '0');
        else
        {
            glutBitmapCharacter(GLUT_BITMAP_HELVETICA_18, ch[k]);
            k++;
        }

    }
}
```

## Counting the score

- This function is used to __display a score__ in a game or application. The function takes in a __character array (string) representing the score, the maximum number of digits that the score can have, an x position, and a y position__ as parameters.

- The function first calculates the number of digits in the score using __strlen().__ It then calculates the __number of leading zeros that need to be displayed before the score using len = numtext - strlen(ch).__ The variable k is used to keep track of the current character in the score string.

- The function _sets the current matrix to the identity matrix using glLoadIdentity()_ and sets the _raster position for the text using glRasterPos2f()._ It then loops through the maximum number of digits specified by numtext.

- For each digit, the function checks if i is less than len. If i is less than len, the function displays a leading zero using glutBitmapCharacter() with the character '0'. If i is greater than or equal to len, the function displays the corresponding digit from the score string using glutBitmapCharacter() with the character ch[k] and increments k.

- Overall, this function can be used to display a score with leading zeros in an OpenGL-based application, such as a game or simulation. The score can be positioned at any x and y coordinates on the screen, and the maximum number of digits for the score can be adjusted by changing the numtext parameter.

-------------
-------------


```
void ovpos()
{
    glClearColor(0, 0, 1, 0);
    for (i = 0; i < 4; i++)
    {
        if (i % 2 == 0)
        {
            if (rand() % 2 == 0)
            {
                ovehicleX[i] = 200;
            }
            else
            {
                ovehicleX[i] = 300;
            }
            ovehicleY[i] = 1000 - i * 160;
        }
        
    }
}
```

## Sets the positions of the obstacles

- This function sets the positions of the obstacles in a game or simulation. The function sets the __background color__ of the screen to blue using __glClearColor().__

- The function then loops through four obstacles using the variable i. For every even i (i.e. 0 and 2), the function __randomly sets the x position of the obstacle using rand() % 2 == 0.__ If the result of this expression is true, the x position of the obstacle is set to __200.__ Otherwise, the x position of the obstacle is set to __300.__ This adds some randomness to the obstacle positions and makes the game more unpredictable.

- After setting the x position of the obstacle, the function sets the __y position of the obstacle using ovehicleY[i] = 1000 - i * 160.__ This places the obstacles at __vertical intervals of 160 pixels, starting at y position 1000__ and decreasing by 160 pixels for each obstacle.
2 Obstacles are set for 1 screen, after which it is called again.

- Note that the function only sets the positions of the even numbered obstacles (i.e. 0 and 2). Overall, this function helps to set up the initial positions of the obstacles in the game or simulation.

---------
---------

```
void drawRoad()
{
    glBegin(GL_QUADS);
    glColor3f(0.5, 0.5, 0.5);
    glVertex2f(250 - 100, 500);
    glVertex2f(250 - 100, 0);
    glVertex2f(250 + 100, 0);
    glVertex2f(250 + 100, 500);
    glEnd();
}

```

## Draws Road

- The drawRoad() function appears to be responsible for drawing a gray rectangle on the screen, which represents the road in the game. The rectangle is drawn using OpenGL's glBegin() and glEnd() functions, which define a sequence of vertices for the shape.

Here's a breakdown of what the code does:

1) glBegin(GL_QUADS) starts the sequence of vertices for a quadrilateral shape.
1) glColor3f(0.5, 0.5, 0.5) sets the color of the shape to a gray color with RGB values of (0.5, 0.5, 0.5).
3) glVertex2f(250 - 100, 500) defines the first vertex of the quadrilateral shape, which is located at (150, 500) on the screen.
4) glVertex2f(250 - 100, 0) defines the second vertex of the quadrilateral shape, which is located at (150, 0) on the screen.
5) glVertex2f(250 + 100, 0) defines the third vertex of the quadrilateral shape, which is located at (350, 0) on the screen.
6) glVertex2f(250 + 100, 500) defines the fourth vertex of the quadrilateral shape, which is located at (350, 500) on the screen.
7) glEnd() ends the sequence of vertices for the shape.
Overall, the drawRoad() function is a simple and straightforward implementation for drawing a rectangle representing a road in a game using OpenGL.


----------
----------

```
void drawDivider()//black patch drawn in middle of road
{
    glLoadIdentity();
    glTranslatef(0, movd, 0);
    for (i = 1; i <= 10; i++)
    {
        glColor3f(0, 0, 0);
        glBegin(GL_QUADS);
        glVertex2f(divx - 5, divy * 15 * i + 18);
        glVertex2f(divx - 5, divy * 15 * i - 18);
        glVertex2f(divx + 5, divy * 15 * i - 18);
        glVertex2f(divx + 5, divy * 15 * i + 18);
        glEnd();
    }
    glLoadIdentity();
}

```

## Black Patch drawn in middle of road

- The drawDivider() function appears to be responsible for drawing black patches on the road that represent the road divider. The function uses OpenGL's glBegin() and glEnd() functions to draw a series of quadrilateral shapes.

Here's a breakdown of what the code does:

1) glLoadIdentity() resets the __modelview matrix to the identity matrix.__
2) glTranslatef(0, movd, 0) __translates the coordinate system along the y-axis by the value of movd, which presumably represents the movement of the road or the player's car.__
3) The for loop iterates 10 times, drawing a quadrilateral shape for each iteration.
4) glColor3f(0, 0, 0) sets the color of the shape to black.
5) glBegin(GL_QUADS) starts the sequence of vertices for a quadrilateral shape.
6) glVertex2f(divx - 5, divy * 15 * i + 18) defines the first vertex of the quadrilateral shape, which is located at (divx - 5, divy * 15 * i + 18) on the screen.
7) glVertex2f(divx - 5, divy * 15 * i - 18) defines the second vertex of the quadrilateral shape, which is located at (divx - 5, divy * 15 * i - 18) on the screen.
8) glVertex2f(divx + 5, divy * 15 * i - 18) defines the third vertex of the quadrilateral shape, which is located at (divx + 5, divy * 15 * i - 18) on the screen.
9) glVertex2f(divx + 5, divy * 15 * i + 18) defines the fourth vertex of the quadrilateral shape, which is located at (divx + 5, divy * 15 * i + 18) on the screen.
10) glEnd() ends the sequence of vertices for the shape.
11) glLoadIdentity() __resets the modelview matrix to the identity matrix.__

- Overall, the drawDivider() function is a simple and effective implementation for drawing black patches on the road to represent the road divider using OpenGL. The function takes into account the movement of the road or player's car, making the game or simulation more realistic.


----------
----------

```
void drawVehicle()//car for racing
{
    glPointSize(10.0);
    glBegin(GL_POINTS);//tire
    glColor3f(0, 0, 0);
    glVertex2f(vehicleX - 25, vehicleY + 16);
    glVertex2f(vehicleX + 25, vehicleY + 16);
    glVertex2f(vehicleX - 25, vehicleY - 16);
    glVertex2f(vehicleX + 25, vehicleY - 16);
    glEnd();

    glBegin(GL_QUADS);
    glColor3f(1, 0, 0);//middle body
    glVertex2f(vehicleX - 25, vehicleY + 20);
    glVertex2f(vehicleX - 25, vehicleY - 20);
    glVertex2f(vehicleX + 25, vehicleY - 20);
    glVertex2f(vehicleX + 25, vehicleY + 20);
    glEnd();

    glBegin(GL_QUADS);//up body
    glColor3f(0, 0, 1);
    glVertex2f(vehicleX - 23, vehicleY + 20);
    glVertex2f(vehicleX - 19, vehicleY + 40);
    glVertex2f(vehicleX + 19, vehicleY + 40);
    glVertex2f(vehicleX + 23, vehicleY + 20);
    glEnd();

    glBegin(GL_QUADS);//down body
    glColor3f(0, 0, 1);
    glVertex2f(vehicleX - 23, vehicleY - 20);
    glVertex2f(vehicleX - 19, vehicleY - 35);
    glVertex2f(vehicleX + 19, vehicleY - 35);
    glVertex2f(vehicleX + 23, vehicleY - 20);
    glEnd();
}

```

## Draw Player's Car

This function draws a vehicle for the game. Here's a breakdown of what it does:

1) It sets the __point size to 10.0__ using __glPointSize(10.0).__
2) It draws four __points for the tires__ using glBegin(GL_POINTS) and glVertex2f(). The points are black and are positioned at the corners of a rectangle centered at the vehicle's position (vehicleX and vehicleY).
3) It draws a rectangle for the middle body of the vehicle using glBegin(GL_QUADS) and glVertex2f(). The rectangle is red and has a __width of 50 and a height of 40__, centered at the vehicle's position.
4) It draws a __trapezoid__ for the upper body of the vehicle using glBegin(GL_QUADS) and glVertex2f(). The trapezoid is blue and is positioned above the middle body. It has a __width of 42 at the top and 38__ at the bottom, and a __height of 20.__
5) It draws a trapezoid for the lower body of the vehicle using glBegin(GL_QUADS) and glVertex2f(). The trapezoid is also blue and is positioned below the middle body. It has a width of 42 at the top and 38 at the bottom, and a __height of 15.__ (smaller than top half)
6) Overall, this function to draw a simple car-like vehicle with a red rectangle for the body and blue trapezoids for the upper and lower parts. The tires are drawn as four black points.


----------
----------

```
void drawOVehicle()//other cars
{

    for (i = 0; i < 4; i++)
    {
        if (i % 2 == 0)
        {
            glPointSize(10.0);
        glBegin(GL_POINTS);//tire
        glColor3f(0, 0, 0);
        glVertex2f(ovehicleX[i] - 25, ovehicleY[i] + 16);
        glVertex2f(ovehicleX[i] + 25, ovehicleY[i] + 16);
        glVertex2f(ovehicleX[i] - 25, ovehicleY[i] - 16);
        glVertex2f(ovehicleX[i] + 25, ovehicleY[i] - 16);
        glEnd();

        glBegin(GL_QUADS);
        glColor3f(0.99609, 0.83984, 0);//middle body
        glVertex2f(ovehicleX[i] - 25, ovehicleY[i] + 20);
        glVertex2f(ovehicleX[i] - 25, ovehicleY[i] - 20);
        glVertex2f(ovehicleX[i] + 25, ovehicleY[i] - 20);
        glVertex2f(ovehicleX[i] + 25, ovehicleY[i] + 20);
        glEnd();

        glBegin(GL_QUADS);//up body
        glColor3f(0, 1, 0);
        glVertex2f(ovehicleX[i] - 23, ovehicleY[i] + 20);
        glVertex2f(ovehicleX[i] - 19, ovehicleY[i] + 40);
        glVertex2f(ovehicleX[i] + 19, ovehicleY[i] + 40);
        glVertex2f(ovehicleX[i] + 23, ovehicleY[i] + 20);
        glEnd();

        glBegin(GL_QUADS);//down body
        glColor3f(0, 1, 0);
        glVertex2f(ovehicleX[i] - 23, ovehicleY[i] - 20);
        glVertex2f(ovehicleX[i] - 19, ovehicleY[i] - 35);
        glVertex2f(ovehicleX[i] + 19, ovehicleY[i] - 35);
        glVertex2f(ovehicleX[i] + 23, ovehicleY[i] - 20);
        glEnd();

        ovehicleY[i] = ovehicleY[i] - 5;

        if (ovehicleY[i] > vehicleY - 25 - 25 && ovehicleY[i] < vehicleY + 25 + 25 && ovehicleX[i] == vehicleX)
        {
            collide = true;
        }

        if (ovehicleY[i] < -25)
        {
            if (rand() % 2 == 0)
            {
                ovehicleX[i] = 200;
            }
            else
            {
                ovehicleX[i] = 300;
            }
            ovehicleY[i] = 600;
        }
        }
        
    }
}
```

## Draw Obstacle Cars

This is a function for drawing multiple other cars on the screen.

1) The function uses a for loop to iterate over each car (2 in this case). For each car, it draws the same design as the player's car, with a black tire, a middle body of yellow, an upper body of green, and a lower body of green.

2) The __car's position is controlled by the ovehicleX and ovehicleY arrays__, which store the x and y coordinates of each car. The ovehicleY value is __updated by subtracting 5 in each iteration of the loop, which causes the car to move downwards on the screen.__

3) The __function also checks for collisions between each other car and the player's car.__ This is done by checking if the __ovehicleY value is within a certain range of the player's car's vehicleY value and if the ovehicleX value is equal to the player's car's vehicleX value.__ If a collision is detected, the __collide variable is set to true.__

4) Finally, __if an other car goes off the bottom of the screen, its position is reset to a random value at the top of the screen.__ The rand() function is used to randomly choose whether the car should appear on the left or right side of the screen.


---------
---------

```
void Specialkey(int key)//allow to use navigation key for movement of car
{
    switch (key)
    {
        
    case GLUT_KEY_UP:
        for (i = 0; i < 4; i++)
        {
            ovehicleY[i] = ovehicleY[i] - 10;
        }
        movd = movd - 20;
        break;
    case GLUT_KEY_DOWN:
        for (i = 0; i < 4; i++)
        {
            ovehicleY[i] = ovehicleY[i] + 10;
        }
        movd = movd + 50;
        break;
    case GLUT_KEY_LEFT:vehicleX = 200;
        break;
    case GLUT_KEY_RIGHT:vehicleX = 300;
        break;

    }

    glutPostRedisplay();
}
```

# Key Control to navigate the Car

- This function is a callback function that handles special key events (e.g. arrow keys) from the user's keyboard. It takes one arguments:

1) key: an integer representing the special key that was pressed (e.g. GLUT_KEY_UP for the up arrow key).(Up,Down,Left,Right)

- If the up arrow key is pressed, the __ovehicleY values for all four other vehicles are decreased by 10,__ and the __movd variable__ (which controls the speed of the background scrolling) __is decreased by 20.__
(Hence the speed of player's car seems to have increased)
- If the down arrow key is pressed, the __ovehicleY values for all four other vehicles are increased by 10, and the movd variable is increased by 50.__
- If the left arrow key is pressed, the __vehicleX variable__ (which controls the x-coordinate of the player's car) __is set to 200.__
- If the right arrow key is pressed, the __vehicleX variable is set to 300.__
Finally, the function calls glutPostRedisplay() to request that the window be redrawn with the new positions of the vehicles and the player's car.


---------
---------

```
void init()
{
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(0, 500, 0, 500);
    glMatrixMode(GL_MODELVIEW);
}
```


- The init() function __sets up the projection matrix and the coordinate system for the scene.__ It sets the projection matrix to the 2D orthographic mode with the __coordinate system ranging from 0 to 500 in both the x and y directions.__ This function is typically called once at the beginning of the program to initialize the graphics system.


-------------
-----------


```
void display()
{

    glClear(GL_COLOR_BUFFER_BIT);
    drawRoad();
    drawDivider();
    drawVehicle();
    drawOVehicle();
    movd = movd - 16;
    if (movd < -60)
    {
        movd = 0;
    }

    score = score + 1;
    glColor3f(1, 1, 1);
    drawText((char*)"Score:", 360, 455);
    _itoa_s(score, buffer, 10);
    drawTextNum(buffer, 6, 420, 455);
    glutSwapBuffers();
    for (q = 0; q <= 20000000; q++) { ; }//delay loop //speed of car
    if (collide == true)
    {
        glColor3f(0, 0, 0);
        drawText((char*)"Game Over", 200, 250);
        glutSwapBuffers();
        getchar();
        //exit(0);
    }
}

```

- The display() function is responsible for rendering the graphics on the screen. In this function, __the road, divider, player car, and other cars are drawn. The score is updated and displayed on the screen.__ A __delay loop__ is used to control the speed of the player car.

- The glClear() function clears the color buffer with the color set in the glClearColor() function in the main() function. The drawRoad(), drawDivider(), drawVehicle(), and drawOVehicle() functions draw the road, divider, player car, and other cars, respectively. 
- The movd variable is used to move the road and divider to create a scrolling effect.
This condition checks if the road has moved past a certain point (-60 in this case), and if it has, it resets the position of the road to 0. This is likely being done to create the illusion of an infinite road where the player can continue driving without ever reaching the end. By resetting the road position, it allows the road and other objects to continue moving without running out of space on the screen.
- The score is updated and displayed on the screen using drawText() and drawTextNum() functions. The glutSwapBuffers() function swaps the front and back buffers to display the rendered scene on the screen.

- If the collide flag is set to true, the game over message is displayed on the screen and the program waits for the user to press a key before exiting.

----------
----------

```
int main(int argc, char** argv)
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_RGB | GLUT_DOUBLE);
    glutInitWindowPosition(100, 100);
    glutInitWindowSize(800, 500);
    glutCreateWindow("2D Car Racing game");
    ovpos();
    init();
    glutDisplayFunc(display);
    glutSpecialFunc(Specialkey);
    glutKeyboardFunc(Normalkey);
    glutIdleFunc(display);
    glutMainLoop();
}
```

- This is the main function of the program. Here, the GLUT library is initialized and a window is created with the title "2D Car Racing game" and the __dimensions of 800x500 pixels.__ The __ovpos function is called to initialize the positions of the obstacles vehicles.__

- The __init function__ is called to set up the projection and modelview matrices.

- The __glutDisplayFunc__ function is used to set the display callback function display which is called whenever the window needs to be redrawn.

- The __glutSpecialFunc and glutKeyboardFunc__ functions are used to set the keyboard callback functions for special and normal keys respectively.

- The __glutIdleFunc function is used to set the idle callback function,__ which is called whenever there are no other events to handle.

- Finally, the __glutMainLoop function is called to start the event processing loop.__