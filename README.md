# Parking Management System

A parking management system that organizes car entry and exit,
tracks parking time, and calculates fees automatically.

## Parking Layout
![Parking Layout](images/parking_layout.png)

## C# Source Code

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        ParkingSystem parking = new ParkingSystem();

        while (true)
        {
            Console.WriteLine("\n(Y) Insert Car  |  (R) Remove Car");
            char choice = Console.ReadLine()[0];

            if (choice == 'Y' || choice == 'y')
                parking.InsertCar();
            else if (choice == 'R' || choice == 'r')
                parking.RemoveCar();
        }
    }
}

// ================= CAR CLASS =================
class Car
{
    public string Code { get; set; }
    public DateTime InTime { get; set; }

    public Car(string code)
    {
        Code = code;
        InTime = DateTime.Now;
    }
}

// ================= PARKING SYSTEM =================
class ParkingSystem
{
    private List<string> AllPlaces = new List<string>();
    private List<Car> ParkedCars = new List<Car>();
    private double CostPerMinute = 2.5;

    public ParkingSystem()
    {
        AddPlaces();
    }

    private void AddPlaces()
    {
        for (int i = 1; i <= 10; i++) AllPlaces.Add("A" + i);
        for (int i = 1; i <= 10; i++) AllPlaces.Add("B" + i);
        for (int i = 1; i <= 10; i++) AllPlaces.Add("C" + i);
    }

    public void InsertCar()
    {
        if (ParkedCars.Count >= AllPlaces.Count)
        {
            Console.WriteLine("Garage is FULL");
            return;
        }

        string place = AllPlaces[ParkedCars.Count];
        Car car = new Car(place);
        ParkedCars.Add(car);

        Console.WriteLine($"Car parked at {place}");
        Console.WriteLine($"Entry Time: {car.InTime}");
    }

    public void RemoveCar()
    {
        Console.Write("Enter Car Code: ");
        string code = Console.ReadLine();

        Car car = ParkedCars.Find(c => c.Code == code);

        if (car == null)
        {
            Console.WriteLine("Car Not Found");
            return;
        }

        double minutes = Math.Ceiling((DateTime.Now - car.InTime).TotalMinutes);
        double cost = minutes * CostPerMinute;

        ParkedCars.Remove(car);

        Console.WriteLine("Car Removed Successfully");
        Console.WriteLine($"Time: {minutes} minutes");
        Console.WriteLine($"Cost: {cost} EGP");
    }
}

