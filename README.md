
#주전자
'''

#include <GL/freeglut.h>

float angle = 0.0f;

void display() {
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);

    glMatrixMode(GL_MODELVIEW);
    glLoadIdentity();
    gluLookAt(0, 0, 3,   // eye
        0, 0, 0,   // center
        0, 1, 0);  // up

    // 주전자의 색깔을 노란색으로 설정 (R=1.0, G=1.0, B=0.0)
    glColor3f(1.0f, 1.0f, 0.0f); 

    // x축을 중심으로 회전 (angle 값에 따라 X축을 기준으로 회전)
    glRotatef(angle, 1.0f, 0.0f, 0.0f); 
    glutWireTeapot(0.5);

    glutSwapBuffers();
}

void timer(int value) {
    angle += 1.0f;
    if (angle > 360.0f) angle -= 360.0f;
    glutPostRedisplay();
    glutTimerFunc(16, timer, 0);
}

void reshape(int w, int h) {
    if (h == 0) h = 1;
    float ratio = (float)w / (float)h;

    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluPerspective(45.0, ratio, 1.0, 100.0);

    glViewport(0, 0, w, h);
}

int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH);
    glutInitWindowSize(800, 600);
    glutCreateWindow("Rotating Yellow Teapot"); // 창 제목 변경

    glEnable(GL_DEPTH_TEST);

    glutDisplayFunc(display);
    glutReshapeFunc(reshape);
    glutTimerFunc(0, timer, 0);

    glutMainLoop();
    return 0;
}
'''
