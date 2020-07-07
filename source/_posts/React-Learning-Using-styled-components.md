---
title: 'React Learning: Using styled-components'
date: 2020-06-09 00:02:50
tags:
- React
- CSS
categories:
- [React, Notes]
---

*React is a mature web framework developed by facebook.*

----------------------------------------

# Installation

    npm install --save styled-components

# Styled Normal HTML Element
for example, `div` tag:

    const StyledDiv = styled.div`
        background: orange;
    `;

    render(
        <StyledDiv>
            Hello World! 
        </StyledDiv>
    );

# Extend Element
for example, `StyledDiv` tag above:

    const SuperStyledDiv = styled(StyledDiv)`
        background: red;
    `;

    render(
        <SuperStyledDiv>
            Hello World! 
        </SuperStyledDiv>
    );

### Check styled-components documentation for more details: https://styled-components.com/docs/basics