---
title: 'React Useful Hooks: useIsMountedRef'
date: 2020-06-16 17:47:11
tags:
- React
- Hooks
categories:
- [React, Useful Hooks]
---

*This hook helps us handle the clean up function in useEffect.*
*Source: https://www.debuggr.io/react-update-unmounted-component/*

----------------------------------------

<!-- more -->

# Usage

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

# Hook
    
    function useIsMountedRef(){
        const isMountedRef = useRef(null);
        useEffect(() => {
            isMountedRef.current = true;
            return () => isMountedRef.current = false;
        });
        return isMountedRef;
    }