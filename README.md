# NMRealmDatebaseAssistant

NMRealmDatebaseAssistant makes it easy for you manipulate (CRUD) your Realm Swift Database. When I worked with [Realm Swift Database](https://realm.io/docs/swift/latest/) for the first time I tried to find a library that would help me manipulate the database without having to write all the time:

```swift
    let realm = try! Realm()
    do {
        try realm.write {
        realm.add(object, update: true) }
    } catch {
        print(“Unable to save object”)
}
```

**or**

```swift
    let realm = try! Realm()
    let objects = realm.objects(entity).sorted(byKeyPath: "name", ascending: true) 
```

Nothing against Realm Swift Database, that in fact is very easy to use, but I just wanted something that would make my code more concise and readable. As I didnʼt find one, I made one for myself. Itʼs very simple and cover only the basic.

## Usage

1. Add "NMRealmDatabaseAssistant.swift" to your project
2. Save and/or delete object(s) after mapping an object

```swift
    // login request code
    .responseObject { (response: DataResponse<Session>) in
        switch response.result {
            case .success: 
                print("Validation Successful")

                NMRealmDatabaseAssistant.deleteAllObjectsFrom(entity: Session.self)
                NMRealmDatabaseAssistant.save(object: response.result.value!)

            case .failure(let error):
                print("Validation Failure: " + error.localizedDescription)
}
```

```swift
    // city request code
    .responseObject { (response: DataResponse<[City]>) in
        switch response.result {
            case .success:
                print("Validation Successful")

                NMRealmDataBaseAssistant.deleteAllObjectsFrom(entity: City.self)
                NMRealmDataBaseAssistant.saveAll(objects: resultArray)

            case .failure(let error):
                print("Validation Failure: " + error.localizedDescription)
}
```

3. Query for all objects from entity (sorted or not)

```swift
class ViewController: UIViewController {
    var cities: [City]!

    override func viewDidLoad() {
        super.viewDidLoad()

        if let data = NMRealmDatabaseAssistant.queryForAnArrayOf(entity: City.self, sortedByKeyPath: "name") {
            self.cities = data
        }
    }
}
```

4. Query for all objects from entity with predicate (sorted or not)

```swift
class ViewController: UIViewController {
    var cities: [City]!

    override func viewDidLoad() {
        super.viewDidLoad()

        let predicate = NSPredicate(format: "estate == %@", estate)
        if let data = NMRealmDatabaseAssistant.queryForAnArrayOf(entity: City.self, withPredicate predicate: predicate, sortedByKeyPath: nil) {
            self.cities = data
        }
    }
}
```

5. Query for a single object from entity

```swift
class ViewController: UIViewController {
    var session: Session!
        override func viewDidLoad() {
            super.viewDidLoad()

            if let session = NMRealmDatabaseAssistant.queryForSingleObjectFrom(entity: Session.self) {
                self.session = session
            }
        }
}
```

6. Query for a single object from entity with a specific identifier

```swift
class ViewController: UIViewController {
    var city: City!

    override func viewDidLoad() {
        super.viewDidLoad()

        if let city = NMRealmDatabaseAssistant.queryForSingleObjectFrom(entity: City.self, withId: 1234) {
            self.city = city
        }
    }
}
```

7. Query for a single object from entity with predicate

```swift
class ViewController: UIViewController {
    var city: City!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        let predicate = NSPredicate(format: "name == %@", name)
        if let city = NMRealmDatabaseAssistant.queryForSingleObjectFrom(entity: City.self, withPredicate: predicate) {
            self.city = city }
        }
}
```

8. Clear database

```swift

    // logout request code
    .responseJSON { (response: DataResponse<Any>) in
        switch response.result {
            case .success:
                print("Validation Successful")
                
                NMRealmDataBaseAssistant.clearDataBase()

            case .failure(let error):
                print("Validation Failure: " + error.localizedDescription)
}
```

## License

[MIT License](https://github.com/nmacambira/NMRealmDatabaseAssistant/blob/master/LICENSE)

## Info

- Swift 3.1

