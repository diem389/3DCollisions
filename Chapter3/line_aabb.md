#Line Against AABB

The code for Line V AABB is almost the same as the code for Line V Sphere. Both functions are wrappers around a ray cast.

This test will __test intersection__, it will __not test containment__. That is, if you have a line, and both start and end points are INSIDE the sphere, the result of this test will be false!

### Implementation

Provide an implementation for the following function

```cs
    public static bool LineTest(Line line, AABB aabb, out Point result)
```

### Unit Test

This unit test will print out errors if any are present in your code.

![S](linecast_test_aabb.png)

```cs
using OpenTK.Graphics.OpenGL;
using Math_Implementation;
using CollisionDetectionSelector.Primitives;

namespace CollisionDetectionSelector.Samples {
    class LinecastAABB : Application {
        public Line[] lines = new Line[] {
            new Line(new Point(2.5f, 2.5f, 2.5f), new Point(4, 4, 4)), // false
            new Line(new Point(1f, 1f, 0f), new Point(0f, 0f, 0f)), // false
            new Line(new Point(-1f, -1f, 0f), new Point(-3f, 0f, 0f)), // true
            new Line(new Point(-5f, 0f, 0f), new Point(5f, 0f, 0f)) // true
        };
        public AABB aabb = new AABB();

        public override void Intialize(int width, int height) {
            GL.Enable(EnableCap.DepthTest);
            GL.PointSize(5f);
            GL.PolygonMode(MaterialFace.FrontAndBack, PolygonMode.Line);

            aabb.Max = new Point(2, 2, 2);
            aabb.Min = new Point(-2, -2, -2);

            bool[] results = new bool[] { false, false, true, true };
            Point result = new Point();
            for (int i = 0; i < results.Length; ++i) {
                if (Collisions.LineTest(lines[i], aabb, out result) != results[i]) {
                    LogError("Line at index " + i + " was " +
                        (results[i] ? "expected" : "not expected") +
                        "to intersect the test aabb");
                }
            }
        }

        public override void Render() {
            base.Render();
            DrawOrigin();

            Point result = new Point();
            foreach (Line line in lines) {
                if (Collisions.LineTest(line, aabb, out result)) {
                    GL.Color3(1f, 0f, 1f);
                    result.Render();
                    GL.Color3(0f, 2f, 0f);
                }
                else {
                    GL.Color3(1f, 0f, 0f);
                }
                line.Render();
            }

            GL.Color3(0f, 0f, 1f);
            aabb.Render();
        }

        private void Log(string s) {
            System.Console.WriteLine(s);
        }
    }
}

```