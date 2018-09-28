# 9.28.1
OpenGL应用：画整圆
// 9.28.2圆.cpp : 定义控制台应用程序的入口点。
//

// 9.28.1.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <GL/glut.h>



void init(void);
void reshape(int w, int h);
void display(void);
void MidCircle(int x0, int y0, int r);
void init(void)
{
}

void reshape(int w, int h)
{
	glViewport(0, 0, w, h);
}

void display(void)
{
	int i;

	glClearColor(0.0f, 0.0f, 0.0f, 0.0f);
	glClear(GL_COLOR_BUFFER_BIT);

	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(-5.0, 45.0, -5.0, 45.0); //窗口坐标范围

	glMatrixMode(GL_MODELVIEW);
	glLoadIdentity();

	//画10*10网格
	glColor3f(0.0f, 1.0f, 0.0f); //绿色
	for (i = 0; i <= 40; i++) //11条水平线
	{
		glBegin(GL_LINES);
		glVertex2d(0.0, i*1.0);
		glVertex2d(40.0, i*1.0);
		glEnd();
	}
	glBegin(GL_LINES); //11条竖线
	for (i = 0; i <= 40; i++)
	{
		glVertex2d(i*1.0, 0.0);
		glVertex2d(i*1.0, 40.0);
	}
	glEnd();

	//在对角线画点
	glColor3f(1.0f, 1.0f, 1.0f); //白色
	glPointSize(5.0f); //点大小
	glBegin(GL_POINTS);
	for (i = 0; i <= 40; i++)
		glVertex2d(i*1.0, i*1.0);
	glEnd();
	for (i = 0; i <= 40; i++)
	{
		glBegin(GL_POINTS);
		glVertex2d(i*1.0, 40.0 - i*1.0);
		glEnd();
	}


	
	////画圆
	MidCircle(15, 15, 9);
	
	glFlush();
}
void MidCircle(int x0, int y0, int r)
{
	int x, y;
	float f;
	x = x0; y = r+x0; f = 1.25 - r; //注意f为下一点（1，r-0.5）的判别式
	while (x <= y)
	{
		glBegin(GL_POINTS);
		glVertex2d(x, y); glVertex2d(2*x0-x, y); glVertex2d(x, 2*y0-y); glVertex2d(2 * x0 - x, 2 * y0 - y);
		glVertex2d(y, x); glVertex2d(y,2 * x0 - x); glVertex2d( 2 * y0 - y,x); glVertex2d(2 * y0 - y, 2 * x0 -x);
		if (f<0)  f += 2 * (x-x0) + 3;
		else { f += 2 * ((x-x0) - (y-y0)) + 5; y--; }
		x++;
	    glEnd();	
	}

}
int main(int argc, char **argv)
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowSize(500, 500);
	glutInitWindowPosition(0, 0);
	glutCreateWindow("GL_0_2d");
	init();

	glutDisplayFunc(display);
	glutReshapeFunc(reshape);
	glutMainLoop();

	return 0;
}
