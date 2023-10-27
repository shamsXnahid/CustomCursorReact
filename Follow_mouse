import type { ComponentType } from "react"
import { useRef, useState, useEffect } from "react"

export function withCursorFollow(Component: ComponentType): ComponentType {
    return (props: any) => {
        const ref = useRef(null)
        const [isHovering, setIsHovering] = useState(false)
        const [position, setPosition] = useState({ left: 0, top: 0 })

        useEffect(() => {
            const handleMouseMove = (e: MouseEvent) => {
                if (!ref.current) return
                const isHovered =
                    e.target instanceof Element &&
                    (e.target === ref.current.parentElement ||
                        ref.current.parentElement.contains(e.target) ||
                        e.target === ref.current ||
                        ref.current.contains(e.target))
                setIsHovering(isHovered)

                if (isHovered) {
                    const parentRect =
                        ref.current.parentElement.getBoundingClientRect()
                    const elementRect = ref.current.getBoundingClientRect()
                    setPosition({
                        left:
                            e.clientX - parentRect.left - elementRect.width / 2,
                        top:
                            e.clientY - parentRect.top - elementRect.height / 2,
                    })
                } else {
                    setIsHovering(false)
                }
            }

            window.addEventListener("mousemove", handleMouseMove)
            return () => {
                window.removeEventListener("mousemove", handleMouseMove)
            }
        }, [])

        return (
            <div
                ref={ref}
                style={{
                    position: "fixed",
                    left: `calc(${position.left}px - 0.1%)`,
                    top: `calc(${position.top}px - 0.1%)`,
                    pointerEvents: "none",
                    transform: "translate(-0.1%, -0.1%)",
                }}
            >
                {isHovering && <Component {...props} />}
            </div>
        )
    }
}
