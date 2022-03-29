# How to write Clean JS

## 1. Use meaningful names

#### Wrong
```
const foo = "JDoe@example.com";
const bar = "John";
const age = 23;
const qux = true;
```

#### Correct
```
const email = "John@example.com";
const firstName = "John";
const age = 23;
const isActive = true
```

## 2. Avoid adding unnecessary contexts

#### Wrong
```
const user = {
  userId: "296e2589-7b33-400a-b762-007b730c8e6d",
  userEmail: "JDoe@example.com",
  userFirstName: "John",
  userLastName: "Doe",
  userAge: 23,
};
```

#### Correct
```
const user = {
  id: "296e2589-7b33-400a-b762-007b730c8e6d",
  email: "JDoe@example.com",
  firstName: "John",
  lastName: "Doe",
  age: 23,
};

user.id;
```

## 3. Avoid hardcoded values

#### Wrong
```
setTimeout(clearSessionData, 900000);
```

#### Correct
```
const SESSION_DURATION_MS = 15 * 60 * 1000;

setTimeout(clearSessionData, SESSION_DURATION_MS);
```

## 4. Use descriptive names

#### Wrong
```
function toggle() {
  // ...
}

function agreed(user) {
  // ...
}
```

#### Correct
```
function toggleThemeSwitcher() {
  // ...
}

function didAgreeToAllTerms(user) {
  // ...
}
```

## 5. Use default arguments

#### Wrong
```
function printAllFilesInDirectory(dir) {
  const directory = dir || "./";
  //   ...
}
```

#### Correct
```
function printAllFilesInDirectory(dir = "./") {
  // ...
}
```

## 6. Limit the number of arguments

#### Wrong
```
function sendPushNotification(title, message, image, isSilent, delayMs) {
  // ...
}

sendPushNotification("New Message", "...", "http://...", false, 1000);
```

#### Correct
```
function sendPushNotification({ title, message, image, isSilent, delayMs }) {
  // ...
}

const notificationConfig = {
  title: "New Message",
  message: "...",
  image: "http://...",
  isSilent: false,
  delayMs: 1000,
};

sendPushNotification(notificationConfig);
```

## 7. Avoid executing multiple actions in a function

#### Wrong
```
function pingUsers(users) {
  users.forEach((user) => {
    const userRecord = database.lookup(user);
    if (!userRecord.isActive()) {
      ping(user);
    }
  });
}
```

#### Correct
```
function pingInactiveUsers(users) {
  users.filter(!isUserActive).forEach(ping);
}

function isUserActive(user) {
  const userRecord = database.lookup(user);
  return userRecord.isActive();
}
```

## 8. Avoid using flags as arguments

#### Wrong
```
function createFile(name, isPublic) {
  if (isPublic) {
    fs.create(`./public/${name}`);
  } else {
    fs.create(name);
  }
}
```

#### Correct
```
function createFile(name) {
  fs.create(name);
}

function createPublicFile(name) {
  createFile(`./public/${name}`);
}
```

## 9. DRY

#### Wrong
```
function renderCarsList(cars) {
  cars.forEach((car) => {
    const price = car.getPrice();
    const make = car.getMake();
    const brand = car.getBrand();
    const nbOfDoors = car.getNbOfDoors();

    render({ price, make, brand, nbOfDoors });
  });
}

function rM4JkQpQtGSRh1R5yf5Nz4aL3VDy8JctSS {
  motorcycles.forEach((motorcycle) => {
    const price = motorcycle.getPrice();
    const make = motorcycle.getMake();
    const brand = motorcycle.getBrand();
    const seatHeight = motorcycle.getSeatHeight();

    render({ price, make, brand, seatHeight });
  });
}
```

#### Correct
```
function rM4JkQpQtGSRh1R5yf5Nz4aL3VDy8JctSS {
  vehicles.forEach((vehicle) => {
    const price = vehicle.getPrice();
    const make = vehicle.getMake();
    const brand = vehicle.getBrand();

    const data = { price, make, brand };

    switch (vehicle.type) {
      case "car":
        data.nbOfDoors = vehicle.getNbOfDoors();
        break;
      case "motorcycle":
        data.seatHeight = vehicle.getSeatHeight();
        break;
    }

    render(data);
  });
}
```

## 10. Avoid callbacks

#### Wrong
```
getUser(function (err, user) {
  getProfile(user, function (err, profile) {
    getAccount(profile, function (err, account) {
      getReports(account, function (err, reports) {
        sendStatistics(reports, function (err) {
          console.error(err);
        });
      });
    });
  });
});
```

### Correct
```
getUser()
  .then(getProfile)
  .then(getAccount)
  .then(getReports)
  .then(sendStatistics)
  .catch((err) => console.error(err));

// or using Async/Await ✅✅

async function sendUserStatistics() {
  try {
    const user = await getUser();
    const profile = await getProfile(user);
    const account = await getAccount(profile);
    const reports = await getReports(account);
    return sendStatistics(reports);
  } catch (e) {
    console.error(err);
  }
}
```
