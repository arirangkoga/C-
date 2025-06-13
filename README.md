# 무지개 사각형

    #pragma comment(lib, "opengl32.lib")
    
    #include <windows.h>
    #include <gl/GL.h>
    
    LRESULT CALLBACK WndProc(HWND h, UINT m, WPARAM w, LPARAM l) {
        if (m == WM_DESTROY) { PostQuitMessage(0); return 0; }
        return DefWindowProc(h, m, w, l);
    }
    
    int WINAPI WinMain(HINSTANCE hInst, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmd) {
        WNDCLASS wc = { CS_OWNDC, WndProc, 0, 0, hInst, 0, 0, 0, 0, L"GL" };
        RegisterClass(&wc);
        HWND hwnd = CreateWindow(L"GL", L"Square", WS_OVERLAPPEDWINDOW, 100, 100, 800, 600, 0, 0, hInst, 0);
        ShowWindow(hwnd, nCmd);
    
        HDC dc = GetDC(hwnd);
        PIXELFORMATDESCRIPTOR pfd = { sizeof(pfd), 1, PFD_DRAW_TO_WINDOW | PFD_SUPPORT_OPENGL | PFD_DOUBLEBUFFER, PFD_TYPE_RGBA };
        SetPixelFormat(dc, ChoosePixelFormat(dc, &pfd), &pfd);
        HGLRC rc = wglCreateContext(dc); wglMakeCurrent(dc, rc);
    
        // Vertices for two triangles forming a square
        // Each row: X, Y, R, G, B
        float v[] = {
            // First triangle
            -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, // Bottom-left (red)
             0.5f, -0.5f, 0.0f, 1.0f, 0.0f, // Bottom-right (green)
            -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, // Top-left (blue)
    
            // Second triangle
             0.5f, -0.5f, 0.0f, 1.0f, 0.0f, // Bottom-right (green)
             0.5f,  0.5f, 1.0f, 1.0f, 0.0f, // Top-right (yellow)
            -0.5f,  0.5f, 0.0f, 0.0f, 1.0f  // Top-left (blue)
        };
    
        MSG msg;
        while (PeekMessage(&msg, 0, 0, 0, PM_REMOVE) || 1) {
            if (msg.message == WM_QUIT) break;
            TranslateMessage(&msg); DispatchMessage(&msg);
    
            glClearColor(0.2f, 0.3f, 0.4f, 1.0f); // A slightly darker background
            glClear(GL_COLOR_BUFFER_BIT);
            glEnableClientState(GL_VERTEX_ARRAY);
            glEnableClientState(GL_COLOR_ARRAY);
    
            glVertexPointer(2, GL_FLOAT, 5 * sizeof(float), v);
            glColorPointer(3, GL_FLOAT, 5 * sizeof(float), v + 2);
            glDrawArrays(GL_TRIANGLES, 0, 6); // Draw 6 vertices (2 triangles)
            SwapBuffers(dc);
        }
    
        wglMakeCurrent(0, 0); wglDeleteContext(rc); ReleaseDC(hwnd, dc);
        return 0;
    }
    
    
    





# 주전자


    
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






# 작성 소감 


## 손충락



이번에 C언어에서 OpenGL을 활용하여 사각형 그리기와 주전자를 3d로 구현하면서 OpenGL의 개념과 구조를 공부 해볼 수 있는 좋은 계기가 되었다. 이 경험이 나중에 게임에서 쓰이는 3d그래픽 프로그래밍을 학습하는데 도움이 되었으면  좋겠다.



## 백승재 



C언어를 이용하여 이러한 활동을 해볼 수 있었다는 것이 아주 좋았고, 졸업을 하게 된다면 게임개발 쪽으로 생각을 하고 있었는데 이렇게 좋은 경험을 쌓을 수 있어서 아주 유익했다.
