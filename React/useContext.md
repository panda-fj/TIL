# useContext

## useContext + useState

```tsx
// provider/UserNameProvider.tsx

"use client"

import React, { createContext, useContext , useState} from 'react'

type userInfoProviderTypes = {
    userName: string,
    setUserName: React.Dispatch<React.SetStateAction<string>>
}

const userInfoProvider = createContext<userInfoProviderTypes | undefined>(undefined);

export default function UserInfoProvider({ children }: { children: React.ReactNode }) {
    const [userName, setUserName] = useState<string>("")

    return (
        <userInfoProvider.Provider value={{ userName, setUserName }}>
                { children }
        </userInfoProvider.Provider>
    )
}

export function useUserInfoContext() {
    const context = useContext(userInfoProvider);
    if (!context) {
        throw new Error("useUserContext must be used within a UserProvider")
    }
    return context
}
```

```tsx
// app/test/page.tsx

import TestDiv1 from '@/components/TestDiv1'
import TestDiv2 from '@/components/TestDiv2'
import UserInfoProvider from '@/provider/UserInfoProvider'

const TestPage = () => {
    return (
        <div className='flex flex-col h-[100vh] w-full items-center justify-center'>
            <UserInfoProvider>
                <TestDiv1/>
                <TestDiv2/>
            </UserInfoProvider>
        </div>
    )
}

export default TestPage
```

```tsx
// conponents/TestDiv1.tsx

"use client"

import { useUserInfoContext } from '@/provider/UserInfoProvider'

const TestDiv1 = () => {
    const {userName, setUserName} = useUserInfoContext();

    return (
        <div className='w-[60%] h-12 bg-slate-500 flex items-center justify-center border-b-[1px]'>
            {userName}
            <button className='p-1 ml-3' onClick={() => setUserName(userName + "+")}>
                UP
            </button>
        </div>
    )
}

export default TestDiv1
```