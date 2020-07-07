---
title: 'React Learning: Clean up in useEffect hook'
date: 2020-06-16 16:50:28
tags:
- React
- Hooks
- Bug
categories:
- [React, Notes]
---

*React is a mature web framework developed by facebook.*

----------------------------------------

# Memory Leak
It sometimes happened that React reports this bug in the console:

    Warning: Can’t perform a React state update on an unmounted component.
    This is a no-op, but it indicates a memory leak in your application.
    To fix, cancel all subscriptions and asynchronous tasks in a useEffect cleanup function.

That is because the component is going to unmount while some states are still waiting for updated.

The way to fix this is to set a flag, for example, called `mounted`, and do the state update only when `mounted` is true. Finally, set it to false when the component is unmount.

# Clean Up In useEffect
useEffect takes a second function which returns a clean up function:

    useEffect(() => {
        // do subscription or state update
        ...

        return () => {
            // clean up
            ...
        };
    });

The return function runs when the component is going to unmount.

# A Better Way: Use Hooks
Custom a hook to handle the clean up function:

    function useIsMountedRef(){
        const isMountedRef = useRef(null);
        useEffect(() => {
            isMountedRef.current = true;
            return () => isMountedRef.current = false;
        });
        return isMountedRef;
    }

and usage:

    function SomeComponent() {
        const isMountedRef = useIsMountedRef();

        useEffect(() => {
            // do something
            ...

            if(isMountedRef.current){
                // do subscription or state update
                ...
            }

            // do something
            ...

        }, [isMountedRef]);

        return (
            // return something
            ...
        );
    }

<!-- more -->

# Reference
S. B. Giat, “React state update on an unmounted component.” [Online]. Available: https://www.debuggr.io/react-update-unmounted-component/.