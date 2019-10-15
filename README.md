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
