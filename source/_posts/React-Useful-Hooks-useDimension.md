---
title: React-Useful-Hooks-useDimension
date: 2020-07-07 22:01:57
tags:
- React
- Hooks
categories:
- [React, Useful Hooks]
---

*This hook helps us get the width and height of an element.*
*Source: From Online*

----------------------------------------

<!-- more -->

# Usage

    //  ref is the reference to the element whose height and width is required
    const divRef = useRef(null);
    const { height, width } = useDimension(divRef);
    ...
    <div ref={divRef}>

# Hook

    import { useRef, useState, useEffect } from "react";
    import ResizeObserver from "resize-observer-polyfill";

    const initialState = { width: 0, height: 0 };
    //  ref is the reference to the element whose height and width is required
    //  const divRef = useRef(null);
    //  const { height, width } = useDimension(divRef);
    //  <div ref={divRef}>
    const useDimension = (ref) => {
        const [dimensions, setdDimensions] = useState(initialState);
        const resizeObserverRef = useRef(null);

        useEffect(() => {
            resizeObserverRef.current = new ResizeObserver((entries = []) => {
                entries.forEach((entry) => {
                    const { width, height } = entry.contentRect;
                    setdDimensions({ width, height });
                });
            });
            if (ref.current) resizeObserverRef.current.observe(ref.current);
            return () => {
                if (resizeObserverRef.current)
                    resizeObserverRef.current.disconnect();
            };
        }, [ref]);
        return dimensions;
    };

    export default useDimension;
