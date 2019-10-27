# Refactoring
# Code Smells
the smells are key signs that refactoring is necessary to enable further developement of the application with equal or greater speed.

# Bloaters
code, methods and classes that have increased to such gargantuan proportions that they are hard to work with.
## Log Method
method conatins too many lines of code (longer that ten lines)
- **Extract Method**
  ```PHP
  function printOwing() {
      $this->printBanner();
      print("name " . $this->name);
      print("amount" . $this->getOutstanding());
  }
  ```
  ```PHP
  function printOwing() {
      $this->printBanner();
      $this->printDetails($this->getOutstanding());
  }

  function printDetails($outstanding) {
      print("name " . $this->name);
      print("amount" . outstanding());
  }
  ```
- Reduce Local Variables and Parameters before extacting a Method using **Replace Temp with Query**
  ```PHP
  $basePrice = $this->quantity * $this->itemPrice;
  if($basePrice > 1000) {
      return $basePrice * 0.95;
  } else {
      return $basePrice * 0.98;
  }
  ```
  ```PHP
  if($this->basePrice() > 1000) {
      return $this->basePrice() * 0.95;
  } else {
      return $this->basePrice() * 0.98;
  }
  function basePrice() {
      return $this->quantity * $this->itemPrice;
  }
  ```
  **Introduce Parameter Object**
  ```PHP
  class Customer {
      amountInvoicedIn(Date $start, Date $end);
      amountReceivedIn(Date $start, Date $end);
      amountOverdueIn(Date $start, Date $end);
  }
  ```
  ```PHP
  class Customer {
      amountInvoicedIn(DateRange $date);
      amountReceivedIn(DateRange $date);
      amountOverdueIn(DateRange $date);
  }
  ```
  **Preserve Whole Object**
  ```PHP
  $low = $daysTempRange->getLow();
  $high = $daysTempRange->getHigh();
  $withinPlan = $plan->withinRange($low, $high);
  ```
  ```PHP
  $withinPlan - $plan->withinRnage($daysTempRange);
  ```
- **Replace Method with Method Object**
  ```PHP
  class Order {
      public function price() {
          $primaryBasePrice = 10;
          $secondaryBasePrice = 20;
          $TertiaryBasePrice = 30;
      }
  }
  ```
  ```PHP
  class Order {
      public function price() {
          return (new PriceCalculator($this))->compute();
      }
  }

  class PriceCalculator {
    private $primaryBasePrice;
    private $secondaryBasePrice;
    private $TertiaryBasePrice;
    public function __construct(Order $order) { }
    public function compute() { }
  }
  ```
- Conditionals and Loops are good clue that can can be mover to a separate method. **Decompose Conditional** for conditionals 
  ```PHP
  if($date->before(SUMMER_START) || $date->after(SUMMER_END)) {
      $charge = $quantity * $winterRate +  $winterServiceCharge;
  } else {
      $charge = $quantity * $summerRate;
  }
  ```
  ```PHP
   if(isSummer($date)) {
      $charge = summerCharge($quantity);
  } else {
      $charge = winterCharge($quantity);
  }
  ```
  **Extract Method** for loops
  ```PHP
  function printProperties($user) {
      for($i=0;$i<$users->size();$i++) {
          $result = "";
          $result .= $user->get($i)->getName();
          $result .= " ";
          $result .= $user->get($i)->getAge();
          echo $result;
      }
  }
  ```
  ```PHP
  function printProperties($user) {
      foreach($users as $user) {
          echo $this->getProperties($user);
      }
  }
  function getProperties($user) {
      return $user->getName()." ".$user->getAge();
  }
  ```




## Large Class
class contains many fields, methods or line of code
- **Extract Class** 
  ```PHP
  class Person {
      private $name;
      private $officeAreaCode;
      private $officeNumber;
      public getTelephoneNumber();
  }
  ```
  ```PHP
  class Person {
      private $name;
      public getTelephoneNubmer;
  }
  class TelephoneNumber {
      private $officeAreaCode;
      private $officeNumber;
      public getTelephoneNumber();
  }
  ```
- **Extract Subclass** 
  ```PHP
  class JobItem {
      public getTotalPrice();
      public getUnitPrice();
      public getEmployee();
  }
  ```
  ```PHP
  class JobItem extends LaborItem {
      public getTotalPrice();
      public getUnitPrice();
      public getEmployee();
  }
  class LaborItem {
      public getUnitPrice();
      public getEmployee();
  }
  ```
- **Extract Interface**
  ```PHP
  class Employee {
      public getRate();
      public hasSpecialSkill();
      public getName();
      public getDepartment();
  }
  ```
  **Extract Interface**
  ```PHP
  interface Billable {
      public getRate();
      public hasSpecialSkill();
  }
  class Employee implements Billable {
      public getRate();
      public hasSpecialSkill();
      public getName();
      public getDepartment();
  }
  ```
- Separate GUI and Domain Data **Duplicate Observed Data**
  ```PHP
  class InteralWindow {
      private TextField $startField;
      private TextField $endField;
      private TextField $lengthField;
      public StartField_FocusLost();
      public EndField_FocusLost();
      public LengthField_FocusLost();
      public calculateLength();
      public calculateEnd();
  }
  ```
  ```PHP
  class InteralWindow {
      private TextField $startField;
      private TextField $endField;
      private TextField $lengthField;
      public StartField_FocusLost();
      public EndField_FocusLost();
      public LengthField_FocusLost();
  }
  class Interval {
      private string $start;
      private string $end;
      private string $length;
      public calculateLength();
      public calcuateEnd();
  }
  ```
## Primitive Obsession
when uses primitive instead of small object, use constant for coding information, use of string constant as field names in data arrays
- **Replace Data Value with Object**
  ```PHP
  class Order {
      string $customer;
  }
  ```
  ```PHP
  class Order {
      Customer $customer;
  }
  class Customer {
      string $name;
  }
  ``` 
- Primitive Fields in Method Parameters
  **Introduce Prameter Object**
  ```PHP
  class Customer {
      public amountInvoicedIn(Date $start, Date $end);
      public amountReceivedIn(Date $start, Date $end);
      public amountOverdueIn(Date $start, Date $end);
  }
  ```
  ```PHP
  class Customer {
      public amountInvoicedIn(Date $dataRange);
      public amountReceivedIn(Date $dataRange);
      public amountOverdueIn(Date $dataRange);
  }
  ```
  or **Preserve Whole Object**
  ```PHP
  $low = $daysTempRange->getLow();
  $high = $daysTempRange->getHigh();
  $withinPlan = $plan->withinRange($low, $high);
  ```
  ```PHP
  $withinPlan = $plan->withinRange($daysTempRange);
  ```
- Replace Type Code with Class when complicated data is coded in variables. 
  **Replace Type Code with Class**
  ```PHP
  class Person {
      int $O;
      int $A;
      int $B;
      int $AB;
      int $bloodgroup;
  }
  ```
  ```PHP
  class Person {

  }
  class BloodGroup {
      BloodGroup $O;
      BloodGroup $A;
      BloodGroup $B;
      BloodGroup $AB;
  }
  ```
  **Replace Type Code with Subclass**
  ```PHP
  class Employee {
      int $engineer;
      int $saleman;
      int $type;
  }
  ```
  ```PHP
  class Employee { }
  class Engineer { }
  class Saleman { }
  ```
  **Replace Type Code with State/Strategy**
  ```PHP
  class Employee {
      int $engineer;
      int $saleman;
      int $type;
  }
  ```
  ```PHP
  class Employee { }
  class EmployeeType { }
  class Engineer { }
  class Saleman { }
  ```
- **Replace Array with Object**
```PHP
$row = [];
$row[0] = "Liverpool";
$row[1] = 15;
```
```PHP
$row = new Performance;
$row->setName("Liverpool");
$row->setWins(15);
```

## Long Parameter List
- **Replace Parameter with Method Call** some of the arguments are just results of method call of another object
  ```PHP
  $basePrice = $this->quantity * $this->itemPrice;
  $seasonDiscount = $this->getSeasonalDiscount();
  $fees = $this->getFees();
  $finalPrice = $this->discountPrice($basePrice, $seasonDiscount, $fees);
  ```
  ```PHP
  $basePrice = $this->quantity * $this->itemPrice;
  $finalPrice = $this->discountPrice($basePrice);
  ```
- **Preserve Whole Object** instead of passing a group of data received from another object as params, pass object itself to the method
- **Introduce Parameter Object** if there are serveral unrelated data element, merage them into single parameter object

_Ignore when cause unwanted dependency between classes_

## Data Clumps
different parts of the code contain identical group of variables (parameters for connecting to database). these clumps should be turned into their own classes
- **Extract Class**
- **Introduce Parameter Object**
- **Preserve Whole Object**

_Ignore when passing entire object on parameters of method instead of primitive type this may create undersirable dependency between two classes_

# Obejct-Orientation Abusers
## Switch Statements
having complex `switch` operator or sequence of `if` statement. _when you see `switch` you should think of **Polymorphism**_
- Isolate switch Operator
**Extract Method**
```PHP
class Order {
    public function calculateTotal() {
        $total = 0;
        foreach($this->getProducts() as $product) {
            $total += $product->quantity * $product->price;
        }
        switch($this->user->getCountry()) {
            case "US": $total *= 0.85; break;
            case "RU": $total *= 0.75; break;
            case "CN": $total *= 0.9; break;
        }
        return $total;
    }
}
```
```PHP
class Order {
    public function calculateTotal() {
        $total = 0;
        foreach($this->getProducts() as $product) {
            $total += $product->quantity * $product->price;
        }
        $total = $this->applyRegionalDiscounts($total);
        return $total;
    }
    public function applyRegionalDiscounts($total) {
        $result = $total;
        switch($this->user->getCountry()) {
            case "US": $result *= 0.85; break;
            case "RU": $result *= 0.75; break;
            case "CN": $result *= 0.9; break;
        }
        return $result;
    }
}
```
**Move Method**
```PHP
class Order {
    public function calculateTotal() {
        $total = 0;
        foreach($this->getProducts() as $product) {
            $total += $product->quantity * $product->price;
        }
        $total = $this->applyRegionalDiscounts($total);
        return $total;
    }
    public function applyRegionalDiscounts($total) {
        $result = $total;
        switch($this->user->getCountry()) {
            case "US": $result *= 0.85; break;
            case "RU": $result *= 0.75; break;
            case "CN": $result *= 0.9; break;
        }
        return $result;
    }
}
```
```PHP
class Order {
    public function calculateTotal() {
        $total = Discount::applyRegionalDiscounts($total, $this->user->getCountry());
        $total = Discount::applyCoupons($total);
    }
}
class Discounts {
    public static function applyRegionalDiscounts($total, $country) {
        $result = $total;
        switch($country) {
            case "US": $result *= 0.85; break;
            case "RU": $result *= 0.75; break;
            case "CN": $result *= 0.9; break;
        }
        return $result;
    }
    public static function applyCoupons($total) {

    }
}
```
- when switch is based on type code, when the program runtime mode is switched **Repleace Type Code with Sublcasses** or **Replace Type Code with State/Strategy**
- **Replace Conditional with Polymorphism**
```PHP
Class Bird {
    public function getSpeed() {
        switch($this->type) {
            case EUROPEAN: return $this->getBaseSpeed();
            case AFRICAN: return $this->getBaseSpeed() - $this->getLoadFactor() * $this->numberOfCocounts;
            case NORWEGIAN_BLUE: return ($this->isNailed) ? 0 : $this->getBaseSpeed($this->voltage);
        }
        throw new Exception("Should be unreachable");
    }
}
```
```PHP
abstract class Bird {
    abstract function getSpeed();
}
class European extends Bird {
    public function getSpeed() {
        return $this->getBaseSpeed();
    }
}
class African extends Bird {
    public function getSpeed() {
        return $this->getBaseSpeed() - $this->getLoadFactor() * $this->numberOfCocounts;
    }
}
class NorwegianBlue extends Bird {
    public function getSpeed() {
        return ($this->isNailed) ? 0 : $this->getBaseSpeed($this->voltage);
    }
}
$speed = $bird->getSpeed();
```
**Replace Type Code with State/Strategy**
```PHP
function setValue($name, $value) {
    if($name === "height") {
        $this->height = $value;
        return;
    }
    if($name === "width") {
        $this->width = $value;
        return;
    }
    assert("Should never reach here");
}
```
```PHP
function setHeight($arg) {
    $this->height = $arg;
}
function setWidth($arg) {
    $this->width = $arg;
}
```
- if one of the conditional option is `null` **Introduce Null Object**
```PHP
if($customer === null) {
    $plan = BillingPlan::basic();
} else {
    $plan = $customer->getPlan();
}
```
```PHP
class NullCustomer extends Customer {
    public function isNull() {
        return true;
    }
    public function getPlan() {
        return new NullPlan();
    }
    // some other NULL functionality
}
// Replace null values with Null-object
$customer = ($order->customer !== null) ? $order->customer : new NullCustomer
// Use Null-object as if it's normal subclass
$plan = $customer->getPlan();
``` 
_Ignore when `switch` operator performs simple action, there's no reason to make code changes_
_often `switch` operators used by factory design pattern to select a created class_

## Temporary Field
get their values only under certain circumstances, otherwise they're empty
- Extract Temporary Fields and Related Behaviors, temporary fields and all code operating on them can be put in a separate class via **Extact Class**, in other words you're creating a method object, archieving the same results as if you would perform **Replace Method with Method Object**
- **Introduce Null object** and integrate it in place of the conditional code which was used to check the temporary field values for existence

## Refused Bequest
if subclass uses only some of methods and properties _inherited_ from its parents, the hierarchy is off-killer. the unneeded method may simply go unsed or be redfined and give off exceptions
- if inheritance make no sense and the subclass really does have nothing in common with superclass, eliminate iheritance in favor of **Replace Inheritance with Delegation** 
- **Extract Superclass** if inheritance is approperiate, get rid of unneeded fields and methods in the subclass. extract all fields and methods needed by the subclass from the paraent class, put them in a new subclass and set both classes to inherit from it
  ```PHP
  class Department {
      public function getTotalAnnualCose();
      public function getName();
      public function getHeadCount();
  }
  class Employee {
      public function getAnnualCose();
      public function getName();
      public function getId();
  }
  ```
  ```PHP
  class Party {
      public function getAnnualCose();
      public function getName();
  }
  class Department extends Party {
      public function getAnnualCose();
      public function getHeadCount();
  }
  class Employee extends Party {
      public function getAnnualCose();
      public function getId();
  }
  ```

## Alternative Classes with Different Interfaces
two classes perform identical functions but have different method names
- **Rename Method** to make them identical in all alternative classes
- Make Methods Signatures and Implementataion Identical. **Move Method** when a method used more in another class thank in its own class, **Add Parameter** when method doesn't have enough data to perform certain actions, **Paramerterize Method** when multiple mehtods perform similar actions that are different only in their internal values, numbers or operations combine these methods and use parameter that pass necessary special value
- Extract Only Identical Parts, if only part of the functionality of the classes is duplicated, use **Extract Superclass**, the existing classes will become subclasses
- **Delete Clones** after determine which treatment method to use and implemented it, you may able to delete one of classes
  
# Change Preventers
need to change something in one place in code, you have to make many changes in other places too.
## Divergent Change
when have to change many unrelated methods when you make changes to a class, when adding new product type and you have to change methods for finding, displaying and ordering products
- **Extract Class**
- Combine classes through inheritance, if different classes have the same behavior combine the classes through inheritance(**Extract Superclass** and **Extract Subclass**)
## Shoutgun Surgery
making any modifications requires that you make small changes to many different classes, single responsibility has been split up among large number of classes
- Consolidate Responsibility in a Single Class, use **Move Method** and **Move Field** to move existing class behaviors into single claass, if there's no class appropriate for this then create a new one
- Remove Redundant Classes, if moving code to the same calss leaves the original classes almost empty, try to get rid of these now-redundant classes via **Inline CLass**
## Parallel Inheritance Hierarchies
whenever you create a subcalss for a class you find yourself needing to create a subclass for another class
- Merge Class Hierarchies (if possible), you may de-duplicate parallel class hierarchies in two steps. first make instances of one hierarchy refer to instances of another hierarchy then remove hierarchy in the referred class by using **Move Method** and **Move Field**

# Dispensables
a dispensable is something pointless and unneeded whose absence would make the code cleaner, more efficient and easier to understand
## Comments
methods filled with explanatory comments, the best comment is a good name for a method or class
- if a comment is intended to explain a complex expression, the experssion should be split into understandable subexpression using **Extract Variable**
  ```PHP
  if(($platform->toUpperCase()->indexIf("MAC") > -1) && ($browser->toUpperCase()->indexOf("IE") > -1 ) && $this->wasInitialized() && $this->resize > 0) {

  }
  ```
  ```PHP
  $isMacOs = $platform->toUpperCase()->indexIf("MAC") > -1;
  $isIE = $browser->toUpperCase()->indexOf("IE") > -1;
  $wasReized = $this->resize > 0;
  if($isMacOs && $isIE && $this->wasInitialized() && $wasResized) {

  }
  ```
- if comment explains a section of code, this section can be turned into a separate method via **Extract Method**, the name of the new method can be taken from the comment text itself
- **Rename Method** if a mehtod has already been extracted but comments still necessary to explain what the method does, give the method a self-explanatory name
- **Introduce Assertion** if you need to assert rules about a state that's necessary for the system to work
  ```PHP
  function getExpenseLimit() {
      //should have either expense limit or a primary project
      return ($this->expenseLimit !== NULL_EXPENSE) ? $this->expenseLimit;
      $this->primaryProject->getMemeberExpenseLimit();
  }
  ```
  ```PHP
  function getExpenseLimit() {
      assert($this->expenseLimit !== NULL_EXPENSE || isset($this->primaryProject));
      return ($this->expenseLimit !== NULL_EXPENSE) ? $this->expenseLimit : $this->primaryProject->getMemberExpenseLimit();
  }
  ```
## Duplicate Code
two code fragments look almost identical
- if the same code is found in two or more methods in the same class use **Extract Method** and place calls for the new method in both places
- if same code is found in two subclasses of the same level use **Extract Method** for both classes followed by **Pull Up Field** for the fields used in the method that you're pulling up
  ```PHP
  class Unit {}
  class Soldier extends Unit {private $health;}
  class Tank extends Unit {private $health;}
  ```
  ```PHP
  class Unit {private $health;}
  class Soldier extends Unit {}
  class Tank extends Unit {}
  ```
- if the same code is found in two subclasses of the same level, if duplicate code is inside constructor use **Pull up Constructor Body**
  ```PHP
  class Manger extends Employee {
      public function __construct($name, $id, $grage) {
          $this->name = $name;
          $this->id = $id;
          $this->grage = $grage;
      }
  }
  ```
  ```PHP
  class Manger extends Employee {
      public function __construct($name, $id, $grage) {
          parent::__construct($name, $id);
          $this->grage = $grage;
      }
  }
  ```
- if same code is found in two subclasses of the same level, if the duplicate code is similar but not completely identical use **Form Template Method**
  ```PHP
  class Site { }
  class ResidentialSite extends Site {
      getBuillableAmount() {
          $base = $this->units * $this->rate;
          $tax = $base * Site::TEX_RATE;
          return $base + $tax;
      }
  }
  class LifelineSite extends Site {
      getBuillableAmount() {
          $base = $this->units * $this->rate * 0.5;
          $tax = $base * Site::TEX_RATE * 0.2;
          return $base + $tax;
      }
  }
  ```
  ```PHP
  class Site {
      getBillableAmount() {
          return $this->getBaseAmount() + $this->getTaxAmount();
      }
      getBaseAmount();
      getTaxAmount();
   }
  class ResidentialSite extends Site {
      getBaseAmount();
      getTaxAmount();
  }
  class LifelineSite extends Site {
      getBaseAmount();
      getTaxAmount();
  }
  ```
- if same code is found in two sublcasses of the same level, if two method do the same thing but use different algorithm, select thte best algrorithm and applay **Subsitute Algorithm**
  ```PHP
  function foundPerson(array $people) {
      for($i=0;$i<count($people);$i++) {
          if($people[$i] === "Don") {
              return "Don";
          }
          if($people[$i] === "John") {
              return "John";
          }
          if($people[$i] === "Kent") {
              return "Kent";
          }
      }
      return "";
  }
  ```
  ```PHP
  function foundPerson(array $people) {
      foreach(["Don", "John", "kent"] as $needle) {
          $id = array_search($needle, $people, true);
          if($id !== false) {
              return $people[$id];
          }
      }
      return "";
  }
  ```
- if duplicate code is found in two different classes, if the classes aren't part of hierrachy use **Extract Superclass** in order to create a single superclass for theses classes that maintains all the previous functionality
- if duplicate code is found in two different classes, if it's difficult or impossible to create a superclass, use **Extract Class** in one class and use the new component in the other
- if large number of conditional expressions are present and perform the same code (differing only in their conditions), merge these operators into a single condition using **Consolidate Conditional Expression** and use **Extract Method** to place the condition in separate method with easy-to-understand name
  ```PHP
  function disabilityAmount() {
      if($this->seniority < 2) {
          return 0;
      }
      if($this->monthsDisabled < 12) {
          return 0;
      }
      if($this->isPartTime) {
          return 0;
      }
      // compute the disability amount
  }
  ```
  ```PHP
  function disabilityAmount() {
      if($this->isNotEligableForDisability()) {
          return 0;
      }
      // compute the disability amount
  }
  ```
- if the same code is performed in all branches of a conidtional expression, place the identical code outside of the condition tree by using **Consolidate Duplicate Conditional Fragments**
  ```PHP
  if(isSpecialDeal()) {
      $total = $price * 0.95;
      send();
  } else {
      $total = $price * 0.98;
      send();
  }
  ```
  ```PHP
  if(isSpecialDeal()) {
      $total = $price * 0.95;
  } else {
      $total = $price * 0.98;
  }
  send();
  ```
## Lazy Class
Understanding and maintaining classes always costs time and money, so if class doesn't do enough to earn your attention it should be deleted
- componets that are near-useless should be give the **Inline Class**
- **Collapse Hierarchy** subclass with few functions
## Data Class
data refers to a class that contains only fields and crude methods for accessing them (getters and setters), these are simply containers for data used by other classes, these classes don't contain any additional functionality and can't independently operate on the data that they own
- if class contains public fields use **Encapsulate Field** to hide them from direct access and require that access be performed via getters and setters only
  ```PHP
  public $name;
  ```
  ```PHP
  private $name;
  public getName() {
      return $this->name;
  }
  public setName($arg) {
      $this->name = $arg;
  }
  ```
- use **Encapsulate Collection** for data stored in collection (such as array)
- review the client code that uses the class, in it you may find functionality that would be better located in the data class itself, if this is the case use **Move Method** and **Extract Method** to migrate this functionality to the data class
- get rid of old data access methods, after the class has been filled with well thought-out methods, you may may want to get rid of old methods for data access that give overly broad access to the class data for this **Remove Setting Method** and **Hide Method**
## Dead Code
variable, parameter, field, method or class is no longer used (usually because it's obsolete)
- delete unused code an uneeded files
- remove unsed classes **Inline class** or **Collapse Hierarchy** can be applied if subclass or superclass is used
- remove unsed parameters using **Remove Prameter**
## Speculative Generality
there's an unused class, method, field or parameter
- **Collapse Hierarchy** for removing unsed abstract classes
- **Inline Class** for unnecessary delegation of functionality to another class can be eliminated
- **Inline Method** for unused methods
- **Remove Paramenter** for methods with unused paramenters
- delete unused fields
# Couplers
all the smells in this group contribute to excessive coupling between classes or show what happens if coupling is replaced by excessive delegation
## Feature Envy
method accesses the data of another object more than its own data
- if method clearly should be moved to another place use **Move Method**
- if only part of method accesses the data of another object, use **Extract Method** to move the part in question
- among two classes pick the one where most data lives, if method uses functions from serveral other classes, first determine which class contains most of the data used. then place the method in this class along with the other data. alternatively use **Extract Method** to split the method into serveral parts that can be placed in different places in different classes
## Inappropriate Intimacy
one class uses the internal fields and methods of another class
- move code to those classes where it's used, the simplest solution is to use **Move Method** and **Move Field** to move parts of one class to the class in which those parts are used, but this works only if the first class truly doesn't need thse parts
- use **Extract Class** and **Hide Delegate** on the class to make the code relations official
- if the classes are mutually interdependent use **Change Bidirectional Association to Unidirectional**
- if this intimacy is between subclass and the superclass, consider **Replace Delegation with Inheritance**
## Message Chains
- to delete a message chain use **Hide Delegate**
- Move Important Code to the Beginning of the Chain, use **Extract Method** and **Move Method**
## Middle Man
- if most of methods classes delegate to another class **Remove Middle Man**
## Incomplete Library Class
- to introduce a few methods to library class use **Introduce Foreign Method**
  ```php
  class Report {
      public function sendReport() {
          $previousDate = clone $this->previousDate;
          $paymentDate = $previousDate->modify("+7 days");
      }
  }
  ```
  ```php
  class Report {
      public function sendReport() {
          $paymentDate = self::nextWeek($this->previousDate);
      }
      private static function nextWeek(DateTime $arg) {
          $previousDate = clone $arg;
          return $previousDate->modify("+7 days");
      }
  }
  ```
- for big changes in class library use **Introduce Local Extension**