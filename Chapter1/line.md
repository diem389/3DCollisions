#Line

Technically a line is the intersection of 2 planes. It runs infinatley in all directions. What we're going to calla  line is actually a __Line Segment__. It's a line between two points. Unlike a line, a line segment does have a length.

Given the above definition, defining a line is pretty simple:

```cs
class Line {
    public Point start;
    public Point end;
}
```

There is really no magic to a line. For example if you want to get the length of a line, it's the same as getting the length of a vector! You get a vector by subtracting start from end, then find the length!

### Code Guide

The ```Line``` class is straight forward. You can follow this guide for implementation, or make your own.

```cs
TODO
```


## On Your Own

Implement the Line class. You can use the code guide above, or use your own implementation

### Sample / Unit Test

You can [Download](../Samples/CollisionLine.rar) the samples for this chapter to see if your result looks like the unit test.

This example is visual only, no errors will be printed to the console if the code is bad. This is what the final render should look like:

![SAMPLE](lines_sample_01.png)

```cs
using OpenTK.Graphics.OpenGL;
using Math_Implementation;
using CollisionDetectionSelector.Primitives;

namespace CollisionDetectionSelector.Samples {
    class PlaneSample : Application {
        protected Vector3 cameraAngle = new Vector3(120.0f, -10f, 20.0f);
        protected float rads = (float)(System.Math.PI / 180.0f);

        Plane[] plane = new Plane[] {
            new Plane(),
            new Plane(),
            new Plane(new Vector3(0f, 5f, 3f), 4f),
            new Plane(new Point(5, 6, 7), new Point(6, 5, 4), new Point(1, 2, 3)),
            new Plane(new Point(0, 0, 2), new Point(1, 1, 2), new Point(2, 0, 2))
        };

        public override void Intialize(int width, int height) {
            //GL.Enable(EnableCap.CullFace);
            GL.Enable(EnableCap.DepthTest);
            GL.PolygonMode(MaterialFace.FrontAndBack, PolygonMode.Fill);
            GL.PointSize(2f);

            plane[1].Normal = new Vector3(1f, 1f, 0f);
            plane[1].Distance = 3.0f;
            plane[3].Distance = 4f;

        }

        public override void Render() {
            Vector3 eyePos = new Vector3();
            eyePos.X = cameraAngle.Z * -(float)System.Math.Sin(cameraAngle.X * rads * (float)System.Math.Cos(cameraAngle.Y * rads));
            eyePos.Y = cameraAngle.Z * -(float)System.Math.Sin(cameraAngle.Y * rads);
            eyePos.Z = -cameraAngle.Z * (float)System.Math.Cos(cameraAngle.X * rads * (float)System.Math.Cos(cameraAngle.Y * rads));

            Matrix4 lookAt = Matrix4.LookAt(eyePos, new Vector3(0.0f, 0.0f, 0.0f), new Vector3(0.0f, 1.0f, 0.0f));
            GL.LoadMatrix(Matrix4.Transpose(lookAt).Matrix);

            DrawOrigin();

            float[][] renderColors = new float[][] {
                new float[] { 1f, 1f, 0f },
                new float[] { 1f, 0f, 1f },
                new float[] { 0f, 1f, 1f },
                new float[] { 1f, 0f, 0f },
                new float[] { 0f, 1f, 0f }
            };

            for (int i = 0; i < plane.Length; ++i) {
                GL.Color3(renderColors[i][0], renderColors[i][1], renderColors[i][2]);
                plane[i].Render();
            }
        }

        public override void Update(float deltaTime) {
            cameraAngle.X += 45.0f * deltaTime;
        }

        protected void DrawOrigin() {
            GL.Begin(PrimitiveType.Lines);
            GL.Color3(1f, 0f, 0f);
            GL.Vertex3(0f, 0f, 0f);
            GL.Vertex3(1f, 0f, 0f);
            GL.Color3(0f, 1f, 0f);
            GL.Vertex3(0f, 0f, 0f);
            GL.Vertex3(0f, 1f, 0f);
            GL.Color3(0f, 0f, 1f);
            GL.Vertex3(0f, 0f, 0f);
            GL.Vertex3(0f, 0f, 1f);
            GL.End();
        }

        public override void Resize(int width, int height) {
            GL.Viewport(0, 0, width, height);
            GL.MatrixMode(MatrixMode.Projection);
            float aspect = (float)width / (float)height;
            Matrix4 perspective = Matrix4.Perspective(60, aspect, 0.01f, 1000.0f);
            GL.LoadMatrix(Matrix4.Transpose(perspective).Matrix);
            GL.MatrixMode(MatrixMode.Modelview);
            GL.LoadIdentity();
        }
    }
}
```

