using System;
using System.Collections.Generic;

namespace PointApp
{
    sealed class Area
    {
        private static readonly Lazy<Area> instance = new Lazy<Area>(() => new Area());

        public int MinX { get; private set; }
        public int MaxX { get; private set; }
        public int MinY { get; private set; }
        public int MaxY { get; private set; }

        private Area()
        {
            MinX = 0;
            MaxX = 100;
            MinY = 0;
            MaxY = 100;
        }

        public static Area Instance => instance.Value;

        public bool Contains(int x, int y)
        {
            return x >= MinX && x <= MaxX && y >= MinY && y <= MaxY;
        }

        public void DisplayInfo()
        {
            Console.WriteLine($"Area boundaries: MinX={MinX}, MaxX={MaxX}, MinY={MinY}, MaxY={MaxY}");
        }
    }

    class Point
    {
        public int X { get; }
        public int Y { get; }

        public Point(int x, int y)
        {
            X = x;
            Y = y;
        }

        public override string ToString()
        {
            return $"Point ({X}, {Y})";
        }

        public static Point CreateRandom(Area area)
        {
            Random rand = new Random();
            int x = rand.Next(area.MinX, area.MaxX + 1);
            int y = rand.Next(area.MinY, area.MaxY + 1);
            return new Point(x, y);
        }
    }

    class Program
    {
        static void Main()
        {
            Area area = Area.Instance;
            area.DisplayInfo();

            int numPoints = 5;
            List<Point> points = new List<Point>();

            for (int i = 0; i < numPoints; i++)
            {
                Point point = Point.CreateRandom(area);

                if (area.Contains(point.X, point.Y))
                {
                    points.Add(point);
                }
            }

            Console.WriteLine("Generated points within the area:");
            points.ForEach(Console.WriteLine);
        }
    }
}
