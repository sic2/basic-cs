# Basic Java

- High-level
- Object-oriented
- Case sensitive

> "Write once, run everywhere"

## Concepts

### Object
An object is a software bundle of related state and behaviour. You can use objects to model real-world objects (e.g. car, house, person, plant, robot, etc.) or abstract concepts (e.g. consumer, producer, listener, etc.). The state of an object is identified by its fields (e.g. name, colour, position, etc). The behaviours, instead, are actions that take place through methods (e.g. move, shoot, turn). 

```
public class Person {
	
	/**
	 * Fields
	 */
	private String name;
	private int age;
	private double money;


	/** 
	 * This is a special method called constructor
	 * The constructor is the method called when a new object is created.
	 * Objects are created using the following syntax:
	 * Person simone = new Person();
	 */
	public Person() {
		name = "Simone";
		age = "24";
		money = "50.6";
	}

	/**
	 * Returns the name of this person.
	 * Name is a String, so we specify that!
	 */
	public String getName() {
		return name;
	}

	public void addMoney(double addend) {
		money = money + addend;
	}

	public void transaction(Bank bank) {
		double tempMoney = bank.getMoney(40);
		addMoney(tempMoney);
	}
}


```

### Class
A class is a blueprint or prototype from which objects are created. In the real world, you'll often find many individual objects all of the same kind. There may be thousands of other bicycles in existence, all of the same make and model. Each bicycle was built from the same set of blueprints and therefore contains the same components. In object-oriented terms, we say that your bicycle is an instance of the class of objects known as bicycles.

Another example of a class:

```
class Bicycle {

    int cadence = 0;
    int speed = 0;
    int gear = 1;

    void changeCadence(int newValue) {
         cadence = newValue;
    }

    void changeGear(int newValue) {
         gear = newValue;
    }

    void speedUp(int increment) {
         speed = speed + increment;   
    }

    void applyBrakes(int decrement) {
         speed = speed - decrement;
    }

    void printStates() {
         System.out.println("cadence:" +
             cadence + " speed:" + 
             speed + " gear:" + gear);
    }
}

```

You can then use this class to create as many instances of a Bicycle as you wish.

```
class BicycleDemo {
    public static void main(String[] args) {

        // Create two different 
        // Bicycle objects
        Bicycle bike1 = new Bicycle();
        Bicycle bike2 = new Bicycle();

        // Invoke methods on 
        // those objects
        bike1.changeCadence(50);
        bike1.speedUp(10);
        bike1.changeGear(2);
        bike1.printStates();

        bike2.changeCadence(50);
        bike2.speedUp(10);
        bike2.changeGear(2);
        bike2.changeCadence(40);
        bike2.speedUp(10);
        bike2.changeGear(3);
        bike2.printStates();
    }
}
```

The output will be:
```
cadence:50 speed:10 gear:2
cadence:40 speed:20 gear:3
```


## Robocode Example

[Robocode API 1.9.2.0](http://robocode.sourceforge.net/docs/robocode/)

![anatomy](http://robowiki.net/w/images/3/30/Anatomy.jpg)

- **Body** - Carries the gun with the radar on top. The body is used for moving the robot ahead and back, as well as turning left or right.
- **Gun** - Mounted on the body and is used for firing energy bullets. The gun can turn left or right.
- **Radar** - Mounted on the gun and is used to scan for other robots when moved. The radar can turn left or right. The radar generates onScannedRobot events when robots are detected. 

``` java
package Django;
import robocode.*;

// This is a comment

/*
 * This is another comment
 * but you can have me in multiple lines!
 */

/**
 * This is a javadoc comment.
 * Franky - a robot by Simone
 */
public class Franky extends Robot {

	private double myEnergy; // This is a field

	/**
	 * This method is called when the Robot thread is initiated
	 * That means, when the battle begins
	 */
	public void run() {
		thisIsACustomMethod(); I am calling a method!

		turnGunRight(120); // Turn the gun right by 120 degrees
		turnGunRight(-30); // Equivalent to turnGunLeft(30);

		myEnergy = getEnergy(); // We get our energy and assign it to a field

		double turningDegrees = 90;
		while (true) { // This will make the while loop run forever, until Robot dies
           ahead(100);
           turnRight(turningDegrees);

           int opponentsLeft = getOthers();
           if (opponentsLeft < 10 && myEnergy < 50) {
           		turningDegrees = 60;
           }

       }
	}

	private void thisIsACustomMethod() {
		// Do some stuff here
	}

	/**
	 * This event is automatically called if there is a robot in range of your radar. 
	 * Note that the robot's radar can only see robot within the range defined by Rules.
	 * RADAR_SCAN_RADIUS (1200 pixels). 
	 */
	public void onScannedRobot(ScannedRobotEvent e) {
		// Assuming radar and gun are aligned

       	if (e.getDistance() < 100) { // this is a boolean expression
           fire(3); // fire is a method of this class. 3 is the energy of fire.
       	} else {
           fire(1);
       	}
	}

	/**
	 * This method is called when your robot your robot is hit by a bullet.
	 */
	public void onHitByBullet(HitByBulletEvent e) {
		out.println("Ouch, that hurts " + e.getName());
	}
	
	/**
	 * This method is called when one of your bullets hits another robot.
	 */
	public void onBulletHit(BulletHitEvent e) {
		out.println("I hit " + e.getName() + "!");
	}

	/**
	 * This method is called when one of your bullets misses, i.e. hits a wall.
	 */
	public void onBulletMissed(BulletMissedEvent e) {
		out.println("Drat, I missed.");
	}

	/**
	 * onHitWall: What to do when you hit a wall
	 */
	public void onHitWall(HitWallEvent e) {
		// This takes away energy from me. I should avoid walls!
	}	

	/**
	 * This method is called when your robot collides with another robot
	 */
	public void onHitRobot(HitRobotEvent e) {
		if (e.getBearing() > -90 && e.getBearing() <= 90) {
           back(100);
       	} else {
           ahead(100);
       	}
	}

	/**
	 * This method is called if your robot dies. 
	 * Actions will have no effect if called from this section. 
	 * The intent is to allow you to perform calculations or print something 
	 * out when the robot is killed. 
	 */
	public void onDeath(DeathEvent e) {
		out.println("I am dead...");
	}

	/**
	 * This method is called when another robot dies. 
	 * You should override it in your robot if you want to be informed of this event.
	 */
	public void onRobotDeath(RobotDeathEvent e) {

	}
}

```

## Links
- [Extra info about the game](robocode-extra.html)
- [Java Oracle concepts](https://docs.oracle.com/javase/tutorial/java/concepts/class.html)
