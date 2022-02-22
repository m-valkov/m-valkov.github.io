---
layout: post
title:  "Type Guard"
date:   2022-02-23 00:23:55 +0300
tags: TypeScript
---

{% highlight typescript %}
let payload: unknown;
const req = {
    body: {
        name: 'mike',
        password: 'password',
    }
};
payload = req.body;


interface IUser {
    name: string;
    password: string;
    role: 'Client' | 'Admin'
}

type UserParams = Pick<IUser, 'name' | 'password'>

// Is a service layer of user entities
const createUser = (user: UserParams) => {
    console.log(user)
} 

const isObject = (value: unknown): value is Record<string, unknown> => {
    return typeof value === 'object' && value !== null;
}

const isValidUserParams = (payload: unknown): payload is UserParams => {
    return isObject(payload) &&
        typeof payload['name'] === 'string' &&
        typeof payload['password'] === 'string' &&
        Object.keys(payload).length === 2
}

if (isValidUserParams(payload)) {
    // call user.service
    createUser(payload)
} else {
    // res.send(400)
}


{% endhighlight %}