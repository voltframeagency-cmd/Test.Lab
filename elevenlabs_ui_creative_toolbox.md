# ElevenLabs UI Creative Developer Toolbox Reference Guide

A complete, structured developer guide for **ElevenLabs UI** (ui.elevenlabs.io) — copy-paste React components for building voice and audio interfaces, including visualizers, real-time orb avatars, speech inputs, and transcript viewers, designed with Framer Motion, Tailwind CSS, Radix, and TypeScript.

## Table of Contents

- [Audio Player](#audio-player)
- [Bar Visualizer](#bar-visualizer)
- [Conversation Bar](#conversation-bar)
- [Conversation](#conversation)
- [Live Waveform](#live-waveform)
- [Matrix](#matrix)
- [Message](#message)
- [Mic Selector](#mic-selector)
- [Orb](#orb)
- [Response](#response)
- [Scrub Bar](#scrub-bar)
- [Shimmering Text](#shimmering-text)
- [Speech Input](#speech-input)
- [Transcript Viewer](#transcript-viewer)
- [Voice Button](#voice-button)
- [Voice Picker](#voice-picker)
- [Waveform](#waveform)

---

### Audio Player <a name="audio-player"></a>

A customizable audio player with progress controls and playback management for music, podcasts, and voice content.

- **Registry Key**: `audio-player`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/audio-player.json
```

#### Dependencies

- **NPM Packages**:
  - `@radix-ui/react-slider`
  - `@radix-ui/react-dropdown-menu`
- **Local Registry Dependencies**:
  - `button`
  - `dropdown-menu`

#### How to Use

##### Example 1

```tsx
<AudioPlayerProvider>{children}</AudioPlayerProvider>
```

##### Example 2

```tsx
interface AudioPlayerItem<TData = unknown> {
  id: string | number
  src: string
  data?: TData
}
```

##### Example 3

```tsx
<AudioPlayerSpeed variant="ghost" size="icon" />
```

##### Example 4

```tsx
<AudioPlayerSpeedButtonGroup speeds={[0.5, 0.75, 1, 1.5, 2]} />
```

##### Example 5

```tsx
const {
  ref, // RefObject<HTMLAudioElement>
  activeItem, // Current playing item
  duration, // Track duration in seconds
  error, // MediaError if any
  isPlaying, // Playing state
  isBuffering, // Buffering state
  playbackRate, // Current playback speed
  isItemActive, // Check if an item is active
  setActiveItem, // Set the active item
  play, // Play audio
  pause, // Pause audio
  seek, // Seek to time
  setPlaybackRate, // Change playback speed
} = useAudioPlayer<TData>()
```

##### Example 6

```tsx
function PlaylistController() {
  const { play, pause, isPlaying, activeItem } = useAudioPlayer()

  const handlePlayNext = () => {
    const nextTrack = getNextTrack(activeItem?.id)
    if (nextTrack) {
      play(nextTrack)
    }
  }

  return <button onClick={handlePlayNext}>Next Track</button>
}
```

##### Example 7

```tsx
const time = useAudioPlayerTime() // Current time in seconds
```

##### Example 8

```tsx
function CustomTimeDisplay() {
  const time = useAudioPlayerTime()
  const { duration } = useAudioPlayer()

  const percentage = duration ? (time / duration) * 100 : 0

  return <div>Progress: {percentage.toFixed(1)}%</div>
}
```

#### Demo Code

##### File: `examples/audio-player-demo.tsx`

```tsx
"use client"

import { PauseIcon, PlayIcon } from "lucide-react"

import { cn } from "@/lib/utils"
import {
  AudioPlayerButton,
  AudioPlayerDuration,
  AudioPlayerProgress,
  AudioPlayerProvider,
  AudioPlayerSpeed,
  AudioPlayerTime,
  exampleTracks,
  useAudioPlayer,
} from "@/components/ui/audio-player"
import { Button } from "@/components/ui/button"
import { Card } from "@/components/ui/card"
import { ScrollArea } from "@/components/ui/scroll-area"

interface Track {
  id: string
  name: string
  url: string
}

export default function AudioPlayer() {
  return (
    <AudioPlayerProvider<Track>>
      <AudioPlayerDemo />
    </AudioPlayerProvider>
  )
}

const AudioPlayerDemo = () => {
  return (
    <Card className="w-full overflow-hidden p-0">
      <div className="flex flex-col lg:h-[180px] lg:flex-row">
        <div className="bg-muted/50 flex flex-col overflow-hidden lg:h-full lg:w-64">
          <ScrollArea className="h-48 w-full lg:h-full">
            <div className="space-y-1 p-3">
              {exampleTracks.map((song, index) => (
                <SongListItem
                  key={song.id}
                  song={song}
                  trackNumber={index + 1}
                />
              ))}
            </div>
          </ScrollArea>
        </div>
        <Player />
      </div>
    </Card>
  )
}

const Player = () => {
  const player = useAudioPlayer<Track>()

  return (
    <div className="flex flex-1 items-center p-4 sm:p-6">
      <div className="mx-auto w-full max-w-2xl">
        <div className="mb-4">
          <h3 className="text-base font-semibold sm:text-lg">
            {player.activeItem?.data?.name ?? "No track selected"}
          </h3>
        </div>
        <div className="flex items-center gap-3 sm:gap-4">
          <AudioPlayerButton
            variant="outline"
            size="default"
            className="h-12 w-12 shrink-0 sm:h-10 sm:w-10"
            disabled={!player.activeItem}
          />
          <div className="flex flex-1 items-center gap-2 sm:gap-3">
            <AudioPlayerTime className="text-xs tabular-nums" />
            <AudioPlayerProgress className="flex-1" />
            <AudioPlayerDuration className="text-xs tabular-nums" />
            <AudioPlayerSpeed variant="ghost" size="icon" />
          </div>
        </div>
      </div>
    </div>
  )
}

const SongListItem = ({
  song,
  trackNumber,
}: {
  song: Track
  trackNumber: number
}) => {
  const player = useAudioPlayer<Track>()
  const isActive = player.isItemActive(song.id)
  const isCurrentlyPlaying = isActive && player.isPlaying

  return (
    <div className="group/song relative">
      <Button
        variant={isActive ? "secondary" : "ghost"}
        size="sm"
        className={cn(
          "h-10 w-full justify-start px-3 font-normal sm:h-9 sm:px-2",
          isActive && "bg-secondary"
        )}
        onClick={() => {
          if (isCurrentlyPlaying) {
            player.pause()
          } else {
            player.play({
              id: song.id,
              src: song.url,
              data: song,
            })
          }
        }}
      >
        <div className="flex w-full items-center gap-3">
          <div className="flex w-5 shrink-0 items-center justify-center">
            {isCurrentlyPlaying ? (
              <PauseIcon className="h-4 w-4 sm:h-3.5 sm:w-3.5" />
            ) : (
              <>
                <span className="text-muted-foreground/60 text-sm tabular-nums group-hover/song:invisible">
                  {trackNumber}
                </span>
                <PlayIcon className="invisible absolute h-4 w-4 group-hover/song:visible sm:h-3.5 sm:w-3.5" />
              </>
            )}
          </div>
          <span className="truncate text-left text-sm">{song.name}</span>
        </div>
      </Button>
    </div>
  )
}
```

#### Props & API Reference

##### `Props` Props

| Prop      | Type                | Default            | Description                    |
| --------- | ------------------- | ------------------ | ------------------------------ |
| speeds    | `readonly number[]` | `[0.5, 1, 1.5, 2]` | Available playback speeds      |
| className | `string`            | -                  | Optional CSS classes           |
| ...props  | `HTMLDivElement`    | -                  | All standard div element props |

#### Component Source Code

##### File: `components/ui/audio-player.tsx`

```tsx
"use client"

import {
  ComponentProps,
  createContext,
  HTMLProps,
  ReactNode,
  RefObject,
  useCallback,
  useContext,
  useEffect,
  useMemo,
  useRef,
  useState,
} from "react"
import * as SliderPrimitive from "@radix-ui/react-slider"
import { Check, PauseIcon, PlayIcon, Settings } from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"

enum ReadyState {
  HAVE_NOTHING = 0,
  HAVE_METADATA = 1,
  HAVE_CURRENT_DATA = 2,
  HAVE_FUTURE_DATA = 3,
  HAVE_ENOUGH_DATA = 4,
}

enum NetworkState {
  NETWORK_EMPTY = 0,
  NETWORK_IDLE = 1,
  NETWORK_LOADING = 2,
  NETWORK_NO_SOURCE = 3,
}

function formatTime(seconds: number) {
  const hrs = Math.floor(seconds / 3600)
  const mins = Math.floor((seconds % 3600) / 60)
  const secs = Math.floor(seconds % 60)

  const formattedMins = mins < 10 ? `0${mins}` : mins
  const formattedSecs = secs < 10 ? `0${secs}` : secs

  return hrs > 0
    ? `${hrs}:${formattedMins}:${formattedSecs}`
    : `${mins}:${formattedSecs}`
}

interface AudioPlayerItem<TData = unknown> {
  id: string | number
  src: string
  data?: TData
}

interface AudioPlayerApi<TData = unknown> {
  ref: RefObject<HTMLAudioElement | null>
  activeItem: AudioPlayerItem<TData> | null
  duration: number | undefined
  error: MediaError | null
  isPlaying: boolean
  isBuffering: boolean
  playbackRate: number
  isItemActive: (id: string | number | null) => boolean
  setActiveItem: (item: AudioPlayerItem<TData> | null) => Promise<void>
  play: (item?: AudioPlayerItem<TData> | null) => Promise<void>
  pause: () => void
  seek: (time: number) => void
  setPlaybackRate: (rate: number) => void
}

const AudioPlayerContext = createContext<AudioPlayerApi<unknown> | null>(null)

export function useAudioPlayer<TData = unknown>(): AudioPlayerApi<TData> {
  const api = useContext(AudioPlayerContext) as AudioPlayerApi<TData> | null
  if (!api) {
    throw new Error(
      "useAudioPlayer cannot be called outside of AudioPlayerProvider"
    )
  }
  return api
}

const AudioPlayerTimeContext = createContext<number | null>(null)

export const useAudioPlayerTime = () => {
  const time = useContext(AudioPlayerTimeContext)
  if (time === null) {
    throw new Error(
      "useAudioPlayerTime cannot be called outside of AudioPlayerProvider"
    )
  }
  return time
}

export function AudioPlayerProvider<TData = unknown>({
  children,
}: {
  children: ReactNode
}) {
  const audioRef = useRef<HTMLAudioElement>(null)
  const itemRef = useRef<AudioPlayerItem<TData> | null>(null)
  const playPromiseRef = useRef<Promise<void> | null>(null)
  const [readyState, setReadyState] = useState<number>(0)
  const [networkState, setNetworkState] = useState<number>(0)
  const [time, setTime] = useState<number>(0)
  const [duration, setDuration] = useState<number | undefined>(undefined)
  const [error, setError] = useState<MediaError | null>(null)
  const [activeItem, _setActiveItem] = useState<AudioPlayerItem<TData> | null>(
    null
  )
  const [paused, setPaused] = useState(true)
  const [playbackRate, setPlaybackRateState] = useState<number>(1)

  const setActiveItem = useCallback(
    async (item: AudioPlayerItem<TData> | null) => {
      if (!audioRef.current) return

      if (item?.id === itemRef.current?.id) {
        return
      }
      itemRef.current = item
      const currentRate = audioRef.current.playbackRate
      audioRef.current.pause()
      audioRef.current.currentTime = 0
      if (item === null) {
        audioRef.current.removeAttribute("src")
      } else {
        audioRef.current.src = item.src
      }
      audioRef.current.load()
      audioRef.current.playbackRate = currentRate
    },
    []
  )

  const play = useCallback(
    async (item?: AudioPlayerItem<TData> | null) => {
      if (!audioRef.current) return

      if (playPromiseRef.current) {
        try {
          await playPromiseRef.current
        } catch (error) {
          console.error("Play promise error:", error)
        }
      }

      if (item === undefined) {
        const playPromise = audioRef.current.play()
        playPromiseRef.current = playPromise
        return playPromise
      }
      if (item?.id === activeItem?.id) {
        const playPromise = audioRef.current.play()
        playPromiseRef.current = playPromise
        return playPromise
      }

      itemRef.current = item
      const currentRate = audioRef.current.playbackRate
      if (!audioRef.current.paused) {
        audioRef.current.pause()
      }
      audioRef.current.currentTime = 0
      if (item === null) {
        audioRef.current.removeAttribute("src")
      } else {
        audioRef.current.src = item.src
      }
      audioRef.current.load()
      audioRef.current.playbackRate = currentRate
      const playPromise = audioRef.current.play()
      playPromiseRef.current = playPromise
      return playPromise
    },
    [activeItem]
  )

  const pause = useCallback(async () => {
    if (!audioRef.current) return

    if (playPromiseRef.current) {
      try {
        await playPromiseRef.current
      } catch (e) {
        console.error(e)
      }
    }

    audioRef.current.pause()
    playPromiseRef.current = null
  }, [])

  const seek = useCallback((time: number) => {
    if (!audioRef.current) return
    audioRef.current.currentTime = time
  }, [])

  const setPlaybackRate = useCallback((rate: number) => {
    if (!audioRef.current) return
    audioRef.current.playbackRate = rate
    setPlaybackRateState(rate)
  }, [])

  const isItemActive = useCallback(
    (id: string | number | null) => {
      return activeItem?.id === id
    },
    [activeItem]
  )

  useAnimationFrame(() => {
    if (audioRef.current) {
      _setActiveItem(itemRef.current)
      setReadyState(audioRef.current.readyState)
      setNetworkState(audioRef.current.networkState)
      setTime(audioRef.current.currentTime)
      setDuration(audioRef.current.duration)
      setPaused(audioRef.current.paused)
      setError(audioRef.current.error)
      setPlaybackRateState(audioRef.current.playbackRate)
    }
  })

  const isPlaying = !paused
  const isBuffering =
    readyState < ReadyState.HAVE_FUTURE_DATA &&
    networkState === NetworkState.NETWORK_LOADING

  const api = useMemo<AudioPlayerApi<TData>>(
    () => ({
      ref: audioRef,
      duration,
      error,
      isPlaying,
      isBuffering,
      activeItem,
      playbackRate,
      isItemActive,
      setActiveItem,
      play,
      pause,
      seek,
      setPlaybackRate,
    }),
    [
      audioRef,
      duration,
      error,
      isPlaying,
      isBuffering,
      activeItem,
      playbackRate,
      isItemActive,
      setActiveItem,
      play,
      pause,
      seek,
      setPlaybackRate,
    ]
  )

  return (
    <AudioPlayerContext.Provider value={api as AudioPlayerApi<unknown>}>
      <AudioPlayerTimeContext.Provider value={time}>
        <audio ref={audioRef} className="hidden" crossOrigin="anonymous" />
        {children}
      </AudioPlayerTimeContext.Provider>
    </AudioPlayerContext.Provider>
  )
}

export const AudioPlayerProgress = ({
  ...otherProps
}: Omit<
  ComponentProps<typeof SliderPrimitive.Root>,
  "min" | "max" | "value"
>) => {
  const player = useAudioPlayer()
  const time = useAudioPlayerTime()
  const wasPlayingRef = useRef(false)

  return (
    <SliderPrimitive.Root
      {...otherProps}
      value={[time]}
      onValueChange={(vals) => {
        player.seek(vals[0])
        otherProps.onValueChange?.(vals)
      }}
      min={0}
      max={player.duration ?? 0}
      step={otherProps.step || 0.25}
      onPointerDown={(e) => {
        wasPlayingRef.current = player.isPlaying
        player.pause()
        otherProps.onPointerDown?.(e)
      }}
      onPointerUp={(e) => {
        if (wasPlayingRef.current) {
          player.play()
        }
        otherProps.onPointerUp?.(e)
      }}
      className={cn(
        "group/player relative flex h-4 touch-none items-center select-none data-[disabled]:opacity-50 data-[orientation=vertical]:h-full data-[orientation=vertical]:min-h-44 data-[orientation=vertical]:w-auto data-[orientation=vertical]:flex-col",
        otherProps.className
      )}
      onKeyDown={(e) => {
        if (e.key === " ") {
          e.preventDefault()
          if (!player.isPlaying) {
            player.play()
          } else {
            player.pause()
          }
        }
        otherProps.onKeyDown?.(e)
      }}
      disabled={
        player.duration === undefined ||
        !Number.isFinite(player.duration) ||
        Number.isNaN(player.duration)
      }
    >
      <SliderPrimitive.Track className="bg-muted relative h-[4px] w-full grow overflow-hidden rounded-full">
        <SliderPrimitive.Range className="bg-primary absolute h-full" />
      </SliderPrimitive.Track>
      <SliderPrimitive.Thumb
        className="relative flex h-0 w-0 items-center justify-center opacity-0 group-hover/player:opacity-100 focus-visible:opacity-100 focus-visible:outline-none disabled:pointer-events-none disabled:opacity-50"
        data-slot="slider-thumb"
      >
        <div className="bg-foreground absolute size-3 rounded-full" />
      </SliderPrimitive.Thumb>
    </SliderPrimitive.Root>
  )
}

export const AudioPlayerTime = ({
  className,
  ...otherProps
}: HTMLProps<HTMLSpanElement>) => {
  const time = useAudioPlayerTime()
  return (
    <span
      {...otherProps}
      className={cn("text-muted-foreground text-sm tabular-nums", className)}
    >
      {formatTime(time)}
    </span>
  )
}

export const AudioPlayerDuration = ({
  className,
  ...otherProps
}: HTMLProps<HTMLSpanElement>) => {
  const player = useAudioPlayer()
  return (
    <span
      {...otherProps}
      className={cn("text-muted-foreground text-sm tabular-nums", className)}
    >
      {player.duration !== null &&
      player.duration !== undefined &&
      !Number.isNaN(player.duration)
        ? formatTime(player.duration)
        : "--:--"}
    </span>
  )
}

interface SpinnerProps {
  className?: string
}

function Spinner({ className }: SpinnerProps) {
  return (
    <div
      className={cn(
        "border-muted border-t-foreground size-3.5 animate-spin rounded-full border-2",
        className
      )}
      role="status"
      aria-label="Loading"
    >
      <span className="sr-only">Loading...</span>
    </div>
  )
}

interface PlayButtonProps extends React.ComponentProps<typeof Button> {
  playing: boolean
  onPlayingChange: (playing: boolean) => void
  loading?: boolean
}

const PlayButton = ({
  playing,
  onPlayingChange,
  className,
  onClick,
  loading,
  ...otherProps
}: PlayButtonProps) => {
  return (
    <Button
      {...otherProps}
      onClick={(e) => {
        onPlayingChange(!playing)
        onClick?.(e)
      }}
      className={cn("relative", className)}
      aria-label={playing ? "Pause" : "Play"}
      type="button"
    >
      {playing ? (
        <PauseIcon
          className={cn("size-4", loading && "opacity-0")}
          aria-hidden="true"
        />
      ) : (
        <PlayIcon
          className={cn("size-4", loading && "opacity-0")}
          aria-hidden="true"
        />
      )}
      {loading && (
        <div className="absolute inset-0 flex items-center justify-center rounded-[inherit] backdrop-blur-xs">
          <Spinner />
        </div>
      )}
    </Button>
  )
}

export interface AudioPlayerButtonProps<TData = unknown>
  extends React.ComponentProps<typeof Button> {
  item?: AudioPlayerItem<TData>
}

export function AudioPlayerButton<TData = unknown>({
  item,
  ...otherProps
}: AudioPlayerButtonProps<TData>) {
  const player = useAudioPlayer<TData>()

  if (!item) {
    return (
      <PlayButton
        {...otherProps}
        playing={player.isPlaying}
        onPlayingChange={(shouldPlay) => {
          if (shouldPlay) {
            player.play()
          } else {
            player.pause()
          }
        }}
        loading={player.isBuffering && player.isPlaying}
      />
    )
  }

  return (
    <PlayButton
      {...otherProps}
      playing={player.isItemActive(item.id) && player.isPlaying}
      onPlayingChange={(shouldPlay) => {
        if (shouldPlay) {
          player.play(item)
        } else {
          player.pause()
        }
      }}
      loading={
        player.isItemActive(item.id) && player.isBuffering && player.isPlaying
      }
    />
  )
}

type Callback = (delta: number) => void

function useAnimationFrame(callback: Callback) {
  const requestRef = useRef<number | null>(null)
  const previousTimeRef = useRef<number | null>(null)
  const callbackRef = useRef<Callback>(callback)

  useEffect(() => {
    callbackRef.current = callback
  }, [callback])

  useEffect(() => {
    const animate = (time: number) => {
      if (previousTimeRef.current !== null) {
        const delta = time - previousTimeRef.current
        callbackRef.current(delta)
      }
      previousTimeRef.current = time
      requestRef.current = requestAnimationFrame(animate)
    }

    requestRef.current = requestAnimationFrame(animate)

    return () => {
      if (requestRef.current) cancelAnimationFrame(requestRef.current)
      previousTimeRef.current = null
    }
  }, [])
}

const PLAYBACK_SPEEDS = [0.25, 0.5, 0.75, 1, 1.25, 1.5, 1.75, 2] as const

export interface AudioPlayerSpeedProps
  extends React.ComponentProps<typeof Button> {
  speeds?: readonly number[]
}

export function AudioPlayerSpeed({
  speeds = PLAYBACK_SPEEDS,
  className,
  variant = "ghost",
  size = "icon",
  ...props
}: AudioPlayerSpeedProps) {
  const player = useAudioPlayer()
  const currentSpeed = player.playbackRate

  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button
          variant={variant}
          size={size}
          className={cn(className)}
          aria-label="Playback speed"
          {...props}
        >
          <Settings className="size-4" />
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent align="end" className="min-w-[120px]">
        {speeds.map((speed) => (
          <DropdownMenuItem
            key={speed}
            onClick={() => player.setPlaybackRate(speed)}
            className="flex items-center justify-between"
          >
            <span className={speed === 1 ? "" : "font-mono"}>
              {speed === 1 ? "Normal" : `${speed}x`}
            </span>
            {currentSpeed === speed && <Check className="size-4" />}
          </DropdownMenuItem>
        ))}
      </DropdownMenuContent>
    </DropdownMenu>
  )
}

export interface AudioPlayerSpeedButtonGroupProps
  extends Omit<React.HTMLAttributes<HTMLDivElement>, "children"> {
  speeds?: readonly number[]
}

export function AudioPlayerSpeedButtonGroup({
  speeds = [0.5, 1, 1.5, 2],
  className,
  ...props
}: AudioPlayerSpeedButtonGroupProps) {
  const player = useAudioPlayer()
  const currentSpeed = player.playbackRate

  return (
    <div
      className={cn("flex items-center gap-1", className)}
      role="group"
      aria-label="Playback speed controls"
      {...props}
    >
      {speeds.map((speed) => (
        <Button
          key={speed}
          variant={currentSpeed === speed ? "default" : "outline"}
          size="sm"
          onClick={() => player.setPlaybackRate(speed)}
          className="min-w-[50px] font-mono text-xs"
        >
          {speed}x
        </Button>
      ))}
    </div>
  )
}

export const exampleTracks = [
  {
    id: "0",
    name: "II - 00",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/00.mp3",
  },
  {
    id: "1",
    name: "II - 01",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/01.mp3",
  },
  {
    id: "2",
    name: "II - 02",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/02.mp3",
  },
  {
    id: "3",
    name: "II - 03",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/03.mp3",
  },
  {
    id: "4",
    name: "II - 04",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/04.mp3",
  },
  {
    id: "5",
    name: "II - 05",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/05.mp3",
  },
  {
    id: "6",
    name: "II - 06",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/06.mp3",
  },
  {
    id: "7",
    name: "II - 07",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/07.mp3",
  },
  {
    id: "8",
    name: "II - 08",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/08.mp3",
  },
  {
    id: "9",
    name: "II - 09",
    url: "https://storage.googleapis.com/eleven-public-cdn/audio/ui-elevenlabs-io/09.mp3",
  },
]
```

---

### Bar Visualizer <a name="bar-visualizer"></a>

Real-time audio frequency visualizer with state-based animations for voice agents and audio interfaces.

- **Registry Key**: `bar-visualizer`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/bar-visualizer.json
```

#### How to Use

##### Example 1

```tsx
<BarVisualizer state="speaking" mediaStream={stream} />
```

##### Example 2

```tsx
const volume = useAudioVolume(mediaStream, {
  fftSize: 32,
  smoothingTimeConstant: 0,
})
```

##### Example 3

```tsx
const frequencyBands = useMultibandVolume(mediaStream, {
  bands: 15,
  loPass: 100,
  hiPass: 200,
  updateInterval: 32,
})
```

##### Example 4

```tsx
const highlightedIndices = useBarAnimator("connecting", 15, 100)
```

##### Example 5

```tsx
type AgentState =
  | "connecting" // Establishing connection
  | "initializing" // Setting up
  | "listening" // Listening for input
  | "speaking" // Playing audio output
  | "thinking" // Processing
```

#### Demo Code

##### File: `examples/bar-visualizer-demo.tsx`

```tsx
"use client"

import { useState } from "react"

import {
  BarVisualizer,
  type AgentState,
} from "@/components/ui/bar-visualizer"
import { Button } from "@/components/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"

export default function BarVisualizerDemo() {
  const [state, setState] = useState<AgentState>("listening")

  return (
    <Card className="">
      <CardHeader>
        <CardTitle>Audio Frequency Visualizer</CardTitle>
        <CardDescription>
          Real-time frequency band visualization with animated state transitions
        </CardDescription>
      </CardHeader>
      <CardContent>
        <div className="space-y-4">
          <BarVisualizer
            state={state}
            demo={true}
            barCount={20}
            minHeight={15}
            maxHeight={90}
            className="h-40 max-w-full"
          />

          <div className="flex flex-wrap gap-2">
            <Button
              size="sm"
              variant={state === "connecting" ? "default" : "outline"}
              onClick={() => setState("connecting")}
            >
              Connecting
            </Button>
            <Button
              size="sm"
              variant={state === "initializing" ? "default" : "outline"}
              onClick={() => setState("initializing")}
            >
              Initializing
            </Button>
            <Button
              size="sm"
              variant={state === "listening" ? "default" : "outline"}
              onClick={() => setState("listening")}
            >
              Listening
            </Button>
            <Button
              size="sm"
              variant={state === "speaking" ? "default" : "outline"}
              onClick={() => setState("speaking")}
            >
              Speaking
            </Button>
            <Button
              size="sm"
              variant={state === "thinking" ? "default" : "outline"}
              onClick={() => setState("thinking")}
            >
              Thinking
            </Button>
          </div>
        </div>
      </CardContent>
    </Card>
  )
}
```

#### Component Source Code

##### File: `components/ui/bar-visualizer.tsx`

```tsx
"use client"

import {
  forwardRef,
  memo,
  useEffect,
  useMemo,
  useRef,
  useState,
  type HTMLAttributes,
} from "react"

import { cn } from "@/lib/utils"

export interface AudioAnalyserOptions {
  fftSize?: number
  smoothingTimeConstant?: number
  minDecibels?: number
  maxDecibels?: number
}

function createAudioAnalyser(
  mediaStream: MediaStream,
  options: AudioAnalyserOptions = {}
) {
  const audioContext = new (window.AudioContext ||
    (window as unknown as { webkitAudioContext: typeof AudioContext })
      .webkitAudioContext)()
  const source = audioContext.createMediaStreamSource(mediaStream)
  const analyser = audioContext.createAnalyser()

  if (options.fftSize) analyser.fftSize = options.fftSize
  if (options.smoothingTimeConstant !== undefined) {
    analyser.smoothingTimeConstant = options.smoothingTimeConstant
  }
  if (options.minDecibels !== undefined)
    analyser.minDecibels = options.minDecibels
  if (options.maxDecibels !== undefined)
    analyser.maxDecibels = options.maxDecibels

  source.connect(analyser)

  const cleanup = () => {
    source.disconnect()
    audioContext.close()
  }

  return { analyser, audioContext, cleanup }
}

/**
 * Hook for tracking the volume of an audio stream using the Web Audio API.
 * @param mediaStream - The MediaStream to analyze
 * @param options - Audio analyser options
 * @returns The current volume level (0-1)
 */
export function useAudioVolume(
  mediaStream?: MediaStream | null,
  options: AudioAnalyserOptions = { fftSize: 32, smoothingTimeConstant: 0 }
) {
  const [volume, setVolume] = useState(0)
  const volumeRef = useRef(0)
  const frameId = useRef<number | undefined>(undefined)

  // Memoize options to prevent unnecessary re-renders
  const memoizedOptions = useMemo(
    () => options,
    [
      options.fftSize,
      options.smoothingTimeConstant,
      options.minDecibels,
      options.maxDecibels,
    ]
  )

  useEffect(() => {
    if (!mediaStream) {
      setVolume(0)
      volumeRef.current = 0
      return
    }

    const { analyser, cleanup } = createAudioAnalyser(
      mediaStream,
      memoizedOptions
    )

    const bufferLength = analyser.frequencyBinCount
    const dataArray = new Uint8Array(bufferLength)
    let lastUpdate = 0
    const updateInterval = 1000 / 30 // 30 FPS

    const updateVolume = (timestamp: number) => {
      if (timestamp - lastUpdate >= updateInterval) {
        analyser.getByteFrequencyData(dataArray)
        let sum = 0
        for (let i = 0; i < dataArray.length; i++) {
          const a = dataArray[i]
          sum += a * a
        }
        const newVolume = Math.sqrt(sum / dataArray.length) / 255

        // Only update state if volume changed significantly
        if (Math.abs(newVolume - volumeRef.current) > 0.01) {
          volumeRef.current = newVolume
          setVolume(newVolume)
        }
        lastUpdate = timestamp
      }
      frameId.current = requestAnimationFrame(updateVolume)
    }

    frameId.current = requestAnimationFrame(updateVolume)

    return () => {
      cleanup()
      if (frameId.current) {
        cancelAnimationFrame(frameId.current)
      }
    }
  }, [mediaStream, memoizedOptions])

  return volume
}

export interface MultiBandVolumeOptions {
  bands?: number
  loPass?: number // Low frequency cutoff
  hiPass?: number // High frequency cutoff
  updateInterval?: number // Update interval in ms
  analyserOptions?: AudioAnalyserOptions
}

const multibandDefaults: MultiBandVolumeOptions = {
  bands: 5,
  loPass: 100,
  hiPass: 600,
  updateInterval: 32,
  analyserOptions: { fftSize: 2048 },
}

// Memoized normalization function to avoid recreating on each render
const normalizeDb = (value: number) => {
  if (value === -Infinity) return 0
  const minDb = -100
  const maxDb = -10
  const db = 1 - (Math.max(minDb, Math.min(maxDb, value)) * -1) / 100
  return Math.sqrt(db)
}

/**
 * Hook for tracking volume across multiple frequency bands
 * @param mediaStream - The MediaStream to analyze
 * @param options - Multiband options
 * @returns Array of volume levels for each frequency band
 */
export function useMultibandVolume(
  mediaStream?: MediaStream | null,
  options: MultiBandVolumeOptions = {}
) {
  const opts = useMemo(
    () => ({ ...multibandDefaults, ...options }),
    [
      options.bands,
      options.loPass,
      options.hiPass,
      options.updateInterval,
      options.analyserOptions?.fftSize,
      options.analyserOptions?.smoothingTimeConstant,
      options.analyserOptions?.minDecibels,
      options.analyserOptions?.maxDecibels,
    ]
  )

  const [frequencyBands, setFrequencyBands] = useState<number[]>(() =>
    new Array(opts.bands).fill(0)
  )
  const bandsRef = useRef<number[]>(new Array(opts.bands).fill(0))
  const frameId = useRef<number | undefined>(undefined)

  useEffect(() => {
    if (!mediaStream) {
      const emptyBands = new Array(opts.bands).fill(0)
      setFrequencyBands(emptyBands)
      bandsRef.current = emptyBands
      return
    }

    const { analyser, cleanup } = createAudioAnalyser(
      mediaStream,
      opts.analyserOptions
    )

    const bufferLength = analyser.frequencyBinCount
    const dataArray = new Float32Array(bufferLength)
    const sliceStart = opts.loPass!
    const sliceEnd = opts.hiPass!
    const sliceLength = sliceEnd - sliceStart
    const chunkSize = Math.ceil(sliceLength / opts.bands!)

    let lastUpdate = 0
    const updateInterval = opts.updateInterval!

    const updateVolume = (timestamp: number) => {
      if (timestamp - lastUpdate >= updateInterval) {
        analyser.getFloatFrequencyData(dataArray)

        // Process directly without creating intermediate arrays
        const chunks = new Array(opts.bands!)

        for (let i = 0; i < opts.bands!; i++) {
          let sum = 0
          let count = 0
          const startIdx = sliceStart + i * chunkSize
          const endIdx = Math.min(sliceStart + (i + 1) * chunkSize, sliceEnd)

          for (let j = startIdx; j < endIdx; j++) {
            sum += normalizeDb(dataArray[j])
            count++
          }

          chunks[i] = count > 0 ? sum / count : 0
        }

        // Only update state if bands changed significantly
        let hasChanged = false
        for (let i = 0; i < chunks.length; i++) {
          if (Math.abs(chunks[i] - bandsRef.current[i]) > 0.01) {
            hasChanged = true
            break
          }
        }

        if (hasChanged) {
          bandsRef.current = chunks
          setFrequencyBands(chunks)
        }

        lastUpdate = timestamp
      }

      frameId.current = requestAnimationFrame(updateVolume)
    }

    frameId.current = requestAnimationFrame(updateVolume)

    return () => {
      cleanup()
      if (frameId.current) {
        cancelAnimationFrame(frameId.current)
      }
    }
  }, [mediaStream, opts])

  return frequencyBands
}

type AnimationState =
  | "connecting"
  | "initializing"
  | "listening"
  | "speaking"
  | "thinking"
  | undefined

export const useBarAnimator = (
  state: AnimationState,
  columns: number,
  interval: number
): number[] => {
  const indexRef = useRef(0)
  const [currentFrame, setCurrentFrame] = useState<number[]>([])
  const animationFrameId = useRef<number | null>(null)

  // Memoize sequence generation
  const sequence = useMemo(() => {
    if (state === "thinking" || state === "listening") {
      return generateListeningSequenceBar(columns)
    } else if (state === "connecting" || state === "initializing") {
      return generateConnectingSequenceBar(columns)
    } else if (state === undefined || state === "speaking") {
      return [new Array(columns).fill(0).map((_, idx) => idx)]
    } else {
      return [[]]
    }
  }, [state, columns])

  useEffect(() => {
    indexRef.current = 0
    setCurrentFrame(sequence[0] || [])
  }, [sequence])

  useEffect(() => {
    let startTime = performance.now()

    const animate = (time: DOMHighResTimeStamp) => {
      const timeElapsed = time - startTime

      if (timeElapsed >= interval) {
        indexRef.current = (indexRef.current + 1) % sequence.length
        setCurrentFrame(sequence[indexRef.current] || [])
        startTime = time
      }

      animationFrameId.current = requestAnimationFrame(animate)
    }

    animationFrameId.current = requestAnimationFrame(animate)

    return () => {
      if (animationFrameId.current !== null) {
        cancelAnimationFrame(animationFrameId.current)
      }
    }
  }, [interval, sequence])

  return currentFrame
}

// Memoize sequence generators
const generateConnectingSequenceBar = (columns: number): number[][] => {
  const seq = []
  for (let x = 0; x < columns; x++) {
    seq.push([x, columns - 1 - x])
  }
  return seq
}

const generateListeningSequenceBar = (columns: number): number[][] => {
  const center = Math.floor(columns / 2)
  const noIndex = -1
  return [[center], [noIndex]]
}

export type AgentState =
  | "connecting"
  | "initializing"
  | "listening"
  | "speaking"
  | "thinking"

export interface BarVisualizerProps extends HTMLAttributes<HTMLDivElement> {
  /** Voice assistant state */
  state?: AgentState
  /** Number of bars to display */
  barCount?: number
  /** Audio source */
  mediaStream?: MediaStream | null
  /** Min/max height as percentage */
  minHeight?: number
  maxHeight?: number
  /** Enable demo mode with fake audio data */
  demo?: boolean
  /** Align bars from center instead of bottom */
  centerAlign?: boolean
}

const BarVisualizerComponent = forwardRef<HTMLDivElement, BarVisualizerProps>(
  (
    {
      state,
      barCount = 15,
      mediaStream,
      minHeight = 20,
      maxHeight = 100,
      demo = false,
      centerAlign = false,
      className,
      style,
      ...props
    },
    ref
  ) => {
    // Audio processing
    const realVolumeBands = useMultibandVolume(mediaStream, {
      bands: barCount,
      loPass: 100,
      hiPass: 200,
    })

    // Generate fake volume data for demo mode using refs to avoid state updates
    const fakeVolumeBandsRef = useRef<number[]>(new Array(barCount).fill(0.2))
    const [fakeVolumeBands, setFakeVolumeBands] = useState<number[]>(() =>
      new Array(barCount).fill(0.2)
    )
    const fakeAnimationRef = useRef<number | undefined>(undefined)

    // Animate fake volume bands for speaking and listening states
    useEffect(() => {
      if (!demo) return

      if (state !== "speaking" && state !== "listening") {
        const bands = new Array(barCount).fill(0.2)
        fakeVolumeBandsRef.current = bands
        setFakeVolumeBands(bands)
        return
      }

      let lastUpdate = 0
      const updateInterval = 50
      const startTime = Date.now() / 1000

      const updateFakeVolume = (timestamp: number) => {
        if (timestamp - lastUpdate >= updateInterval) {
          const time = Date.now() / 1000 - startTime
          const newBands = new Array(barCount)

          for (let i = 0; i < barCount; i++) {
            const waveOffset = i * 0.5
            const baseVolume = Math.sin(time * 2 + waveOffset) * 0.3 + 0.5
            const randomNoise = Math.random() * 0.2
            newBands[i] = Math.max(0.1, Math.min(1, baseVolume + randomNoise))
          }

          // Only update if values changed significantly
          let hasChanged = false
          for (let i = 0; i < barCount; i++) {
            if (Math.abs(newBands[i] - fakeVolumeBandsRef.current[i]) > 0.05) {
              hasChanged = true
              break
            }
          }

          if (hasChanged) {
            fakeVolumeBandsRef.current = newBands
            setFakeVolumeBands(newBands)
          }

          lastUpdate = timestamp
        }

        fakeAnimationRef.current = requestAnimationFrame(updateFakeVolume)
      }

      fakeAnimationRef.current = requestAnimationFrame(updateFakeVolume)

      return () => {
        if (fakeAnimationRef.current) {
          cancelAnimationFrame(fakeAnimationRef.current)
        }
      }
    }, [demo, state, barCount])

    // Use fake or real volume data based on demo mode
    const volumeBands = useMemo(
      () => (demo ? fakeVolumeBands : realVolumeBands),
      [demo, fakeVolumeBands, realVolumeBands]
    )

    // Animation sequencing
    const highlightedIndices = useBarAnimator(
      state,
      barCount,
      state === "connecting"
        ? 2000 / barCount
        : state === "thinking"
          ? 150
          : state === "listening"
            ? 500
            : 1000
    )

    return (
      <div
        ref={ref}
        data-state={state}
        className={cn(
          "relative flex justify-center gap-1.5",
          centerAlign ? "items-center" : "items-end",
          "bg-muted h-32 w-full overflow-hidden rounded-lg p-4",
          className
        )}
        style={{
          ...style,
        }}
        {...props}
      >
        {volumeBands.map((volume, index) => {
          const heightPct = Math.min(
            maxHeight,
            Math.max(minHeight, volume * 100 + 5)
          )
          const isHighlighted = highlightedIndices?.includes(index) ?? false

          return (
            <Bar
              key={index}
              heightPct={heightPct}
              isHighlighted={isHighlighted}
              state={state}
            />
          )
        })}
      </div>
    )
  }
)

// Memoized Bar component to prevent unnecessary re-renders
const Bar = memo<{
  heightPct: number
  isHighlighted: boolean
  state?: AgentState
}>(({ heightPct, isHighlighted, state }) => (
  <div
    data-highlighted={isHighlighted}
    className={cn(
      "max-w-[12px] min-w-[8px] flex-1 transition-all duration-150",
      "rounded-full",
      "bg-border data-[highlighted=true]:bg-primary",
      state === "speaking" && "bg-primary",
      state === "thinking" && isHighlighted && "animate-pulse"
    )}
    style={{
      height: `${heightPct}%`,
      animationDuration: state === "thinking" ? "300ms" : undefined,
    }}
  />
))

Bar.displayName = "Bar"

// Wrap the main component with memo for prop comparison optimization
const BarVisualizer = memo(BarVisualizerComponent, (prevProps, nextProps) => {
  return (
    prevProps.state === nextProps.state &&
    prevProps.barCount === nextProps.barCount &&
    prevProps.mediaStream === nextProps.mediaStream &&
    prevProps.minHeight === nextProps.minHeight &&
    prevProps.maxHeight === nextProps.maxHeight &&
    prevProps.demo === nextProps.demo &&
    prevProps.centerAlign === nextProps.centerAlign &&
    prevProps.className === nextProps.className &&
    JSON.stringify(prevProps.style) === JSON.stringify(nextProps.style)
  )
})

BarVisualizerComponent.displayName = "BarVisualizerComponent"
BarVisualizer.displayName = "BarVisualizer"

export { BarVisualizer }
```

---

### Conversation Bar <a name="conversation-bar"></a>

A complete voice conversation interface with microphone controls, text input, and real-time waveform visualization for ElevenLabs agents.

- **Registry Key**: `conversation-bar`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/conversation-bar.json
```

#### Dependencies

- **NPM Packages**:
  - `@elevenlabs/react`
- **Local Registry Dependencies**:
  - `button`
  - `https://ui.elevenlabs.io/r/live-waveform.json`
  - `card`
  - `separator`
  - `textarea`

#### Demo Code

##### File: `examples/conversation-bar-demo.tsx`

```tsx
"use client"

import { ConversationProvider } from "@elevenlabs/react"

import { ConversationBar } from "@/components/ui/conversation-bar"

const DEFAULT_AGENT = {
  agentId: process.env.NEXT_PUBLIC_ELEVENLABS_AGENT_ID!,
}

export default function ConversationBarDemo() {
  return (
    <ConversationProvider>
      <div className="flex min-h-[200px] w-full items-center justify-center">
        <div className="w-full max-w-md">
          <ConversationBar agentId={DEFAULT_AGENT.agentId} />
        </div>
      </div>
    </ConversationProvider>
  )
}
```

#### Component Source Code

##### File: `components/ui/conversation-bar.tsx`

```tsx
"use client"

import * as React from "react"
import {
  useConversationControls,
  useConversationInput,
  useConversationStatus,
} from "@elevenlabs/react"
import {
  ArrowUpIcon,
  ChevronDown,
  Keyboard,
  Mic,
  MicOff,
  PhoneIcon,
  XIcon,
} from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Card } from "@/components/ui/card"
import { LiveWaveform } from "@/components/ui/live-waveform"
import { Separator } from "@/components/ui/separator"
import { Textarea } from "@/components/ui/textarea"

export interface ConversationBarProps {
  /**
   * ElevenLabs Agent ID to connect to
   */
  agentId: string

  /**
   * Custom className for the container
   */
  className?: string

  /**
   * Custom className for the waveform
   */
  waveformClassName?: string

  /**
   * Callback when user sends a message
   */
  onSendMessage?: (message: string) => void
}

export const ConversationBar = React.forwardRef<
  HTMLDivElement,
  ConversationBarProps
>(({ agentId, className, waveformClassName, onSendMessage }, ref) => {
  const { status } = useConversationStatus()
  const { startSession, endSession, sendUserMessage, sendContextualUpdate } =
    useConversationControls()
  const { isMuted, setMuted } = useConversationInput()
  const [keyboardOpen, setKeyboardOpen] = React.useState(false)
  const [textInput, setTextInput] = React.useState("")
  const mediaStreamRef = React.useRef<MediaStream | null>(null)

  const isConnected = status === "connected"

  const getMicStream = React.useCallback(async () => {
    if (mediaStreamRef.current) return mediaStreamRef.current

    const stream = await navigator.mediaDevices.getUserMedia({ audio: true })
    mediaStreamRef.current = stream

    return stream
  }, [])

  const startConversation = React.useCallback(async () => {
    try {
      await getMicStream()

      startSession({
        agentId,
        connectionType: "webrtc",
      })
    } catch (error) {
      console.error("Error starting conversation:", error)
    }
  }, [getMicStream, agentId, startSession])

  const handleEndSession = React.useCallback(() => {
    endSession()

    if (mediaStreamRef.current) {
      mediaStreamRef.current.getTracks().forEach((t) => t.stop())
      mediaStreamRef.current = null
    }
  }, [endSession])

  const toggleMute = React.useCallback(() => {
    setMuted(!isMuted)
  }, [isMuted, setMuted])

  const handleStartOrEnd = React.useCallback(() => {
    if (status === "connected" || status === "connecting") {
      handleEndSession()
    } else if (status === "disconnected" || status === "error") {
      startConversation()
    }
  }, [status, handleEndSession, startConversation])

  const handleSendText = React.useCallback(() => {
    if (!textInput.trim()) return

    const messageToSend = textInput
    sendUserMessage(messageToSend)
    setTextInput("")
    onSendMessage?.(messageToSend)
  }, [sendUserMessage, textInput, onSendMessage])

  const handleTextChange = React.useCallback(
    (e: React.ChangeEvent<HTMLTextAreaElement>) => {
      const value = e.target.value
      setTextInput(value)

      if (value.trim() && isConnected) {
        sendContextualUpdate(value)
      }
    },
    [sendContextualUpdate, isConnected]
  )

  const handleKeyDown = React.useCallback(
    (e: React.KeyboardEvent<HTMLTextAreaElement>) => {
      if (e.key === "Enter" && !e.shiftKey) {
        e.preventDefault()
        handleSendText()
      }
    },
    [handleSendText]
  )

  React.useEffect(() => {
    return () => {
      if (mediaStreamRef.current) {
        mediaStreamRef.current.getTracks().forEach((t) => t.stop())
      }
    }
  }, [])

  return (
    <div
      ref={ref}
      className={cn("flex w-full items-end justify-center p-4", className)}
    >
      <Card className="m-0 w-full gap-0 border p-0 shadow-lg">
        <div className="flex flex-col-reverse">
          <div>
            {keyboardOpen && <Separator />}
            <div className="flex items-center justify-between gap-2 p-2">
              <div className="h-8 w-[120px] md:h-10">
                <div
                  className={cn(
                    "flex h-full items-center gap-2 rounded-md py-1",
                    "bg-foreground/5 text-foreground/70"
                  )}
                >
                  <div className="h-full flex-1">
                    <div
                      className={cn(
                        "relative flex h-full w-full shrink-0 items-center justify-center overflow-hidden rounded-sm",
                        waveformClassName
                      )}
                    >
                      <LiveWaveform
                        key={
                          status === "disconnected" || status === "error"
                            ? "idle"
                            : "active"
                        }
                        active={isConnected && !isMuted}
                        processing={status === "connecting"}
                        barWidth={3}
                        barGap={1}
                        barRadius={4}
                        fadeEdges={true}
                        fadeWidth={24}
                        sensitivity={1.8}
                        smoothingTimeConstant={0.85}
                        height={20}
                        mode="static"
                        className={cn(
                          "h-full w-full transition-opacity duration-300",
                          (status === "disconnected" || status === "error") &&
                            "opacity-0"
                        )}
                      />
                      {(status === "disconnected" || status === "error") && (
                        <div className="absolute inset-0 flex items-center justify-center">
                          <span className="text-foreground/50 text-[10px] font-medium">
                            Customer Support
                          </span>
                        </div>
                      )}
                    </div>
                  </div>
                </div>
              </div>
              <div className="flex items-center">
                <Button
                  variant="ghost"
                  size="icon"
                  onClick={toggleMute}
                  aria-pressed={isMuted}
                  className={cn(isMuted ? "bg-foreground/5" : "")}
                  disabled={!isConnected}
                >
                  {isMuted ? <MicOff /> : <Mic />}
                </Button>
                <Button
                  variant="ghost"
                  size="icon"
                  onClick={() => setKeyboardOpen((v) => !v)}
                  aria-pressed={keyboardOpen}
                  className="relative"
                  disabled={!isConnected}
                >
                  <Keyboard
                    className={
                      "h-5 w-5 transform-gpu transition-all duration-200 ease-[cubic-bezier(0.22,1,0.36,1)] " +
                      (keyboardOpen
                        ? "scale-75 opacity-0"
                        : "scale-100 opacity-100")
                    }
                  />
                  <ChevronDown
                    className={
                      "absolute inset-0 m-auto h-5 w-5 transform-gpu transition-all delay-50 duration-200 ease-[cubic-bezier(0.34,1.56,0.64,1)] " +
                      (keyboardOpen
                        ? "scale-100 opacity-100"
                        : "scale-75 opacity-0")
                    }
                  />
                </Button>
                <Separator orientation="vertical" className="mx-1 -my-2.5" />
                <Button variant="ghost" size="icon" onClick={handleStartOrEnd}>
                  {isConnected || status === "connecting" ? (
                    <XIcon className="h-5 w-5" />
                  ) : (
                    <PhoneIcon className="h-5 w-5" />
                  )}
                </Button>
              </div>
            </div>
          </div>

          <div
            className={cn(
              "overflow-hidden transition-all duration-300 ease-out",
              keyboardOpen ? "max-h-[120px]" : "max-h-0"
            )}
          >
            <div className="relative px-2 pt-2 pb-2">
              <Textarea
                value={textInput}
                onChange={handleTextChange}
                onKeyDown={handleKeyDown}
                placeholder="Enter your message..."
                className="min-h-[100px] resize-none border-0 pr-12 shadow-none focus-visible:ring-0"
                disabled={!isConnected}
              />
              <Button
                size="icon"
                variant="ghost"
                onClick={handleSendText}
                disabled={!textInput.trim() || !isConnected}
                className="absolute right-3 bottom-3 h-8 w-8"
              >
                <ArrowUpIcon className="h-4 w-4" />
              </Button>
            </div>
          </div>
        </div>
      </Card>
    </div>
  )
})

ConversationBar.displayName = "ConversationBar"
```

---

### Conversation <a name="conversation"></a>

A scrolling conversation container with auto-scroll and sticky-to-bottom behavior for chat interfaces.

- **Registry Key**: `conversation`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/conversation.json
```

#### Dependencies

- **NPM Packages**:
  - `use-stick-to-bottom`
- **Local Registry Dependencies**:
  - `button`

#### Demo Code

##### File: `examples/conversation-demo.tsx`

```tsx
"use client"

import { useEffect, useState } from "react"

import { Card } from "@/components/ui/card"
import {
  Conversation,
  ConversationContent,
  ConversationEmptyState,
  ConversationScrollButton,
} from "@/components/ui/conversation"
import { Message, MessageContent } from "@/components/ui/message"
import { Orb } from "@/components/ui/orb"
import { Response } from "@/components/ui/response"

const allMessages = [
  {
    id: "1",
    role: "user" as const,
    parts: [
      {
        type: "text",
        text: "Hey, I need help with my order",
      },
    ],
  },
  {
    id: "2",
    role: "assistant" as const,
    parts: [
      {
        type: "text",
        tokens: [
          "Hi!",
          " I'd",
          " be",
          " happy",
          " to",
          " help",
          " you",
          " with",
          " your",
          " order.",
          " Could",
          " you",
          " please",
          " provide",
          " your",
          " order",
          " number?",
        ],
        text: "Hi! I'd be happy to help you with your order. Could you please provide your order number?",
      },
    ],
  },
  {
    id: "3",
    role: "user" as const,
    parts: [
      {
        type: "text",
        text: "It's ORDER-12345",
      },
    ],
  },
  {
    id: "4",
    role: "assistant" as const,
    parts: [
      {
        type: "text",
        tokens: [
          "Thank",
          " you!",
          " Let",
          " me",
          " pull",
          " up",
          " your",
          " order",
          " details.",
          " I",
          " can",
          " see",
          " that",
          " your",
          " order",
          " was",
          " placed",
          " on",
          " March",
          " 15th",
          " and",
          " is",
          " currently",
          " being",
          " processed.",
          " It",
          " should",
          " ship",
          " within",
          " the",
          " next",
          " 1-2",
          " business",
          " days.",
          " Is",
          " there",
          " anything",
          " specific",
          " you'd",
          " like",
          " to",
          " know",
          " about",
          " this",
          " order?",
        ],
        text: "Thank you! Let me pull up your order details. I can see that your order was placed on March 15th and is currently being processed. It should ship within the next 1-2 business days. Is there anything specific you'd like to know about this order?",
      },
    ],
  },
  {
    id: "5",
    role: "user" as const,
    parts: [
      {
        type: "text",
        text: "Can I change the shipping address?",
      },
    ],
  },
  {
    id: "6",
    role: "assistant" as const,
    parts: [
      {
        type: "text",
        tokens: [
          "Absolutely!",
          " Since",
          " the",
          " order",
          " hasn't",
          " shipped",
          " yet,",
          " I",
          " can",
          " update",
          " the",
          " shipping",
          " address",
          " for",
          " you.",
          " What",
          " would",
          " you",
          " like",
          " the",
          " new",
          " address",
          " to",
          " be?",
        ],
        text: "Absolutely! Since the order hasn't shipped yet, I can update the shipping address for you. What would you like the new address to be?",
      },
    ],
  },
]

const ConversationDemo = () => {
  const [messages, setMessages] = useState<typeof allMessages>([])
  const [streamingMessageIndex, setStreamingMessageIndex] = useState<
    number | null
  >(null)
  const [streamingContent, setStreamingContent] = useState("")

  useEffect(() => {
    const timeouts: NodeJS.Timeout[] = []
    const intervals: NodeJS.Timeout[] = []
    let currentMessageIndex = 0

    const addNextMessage = () => {
      if (currentMessageIndex >= allMessages.length) return

      const message = allMessages[currentMessageIndex]
      const part = message.parts[0]

      if (message.role === "assistant" && "tokens" in part && part.tokens) {
        setStreamingMessageIndex(currentMessageIndex)
        setStreamingContent("")

        let currentContent = ""
        let tokenIndex = 0

        const streamInterval = setInterval(() => {
          if (tokenIndex < part.tokens.length) {
            currentContent += part.tokens[tokenIndex]
            setStreamingContent(currentContent)
            tokenIndex++
          } else {
            clearInterval(streamInterval)
            setMessages((prev) => [...prev, message])
            setStreamingMessageIndex(null)
            setStreamingContent("")
            currentMessageIndex++

            // Add next message after a delay
            timeouts.push(setTimeout(addNextMessage, 500))
          }
        }, 100)

        intervals.push(streamInterval)
      } else {
        setMessages((prev) => [...prev, message])
        currentMessageIndex++

        timeouts.push(setTimeout(addNextMessage, 800))
      }
    }

    // Start after 1 second
    timeouts.push(setTimeout(addNextMessage, 1000))

    return () => {
      timeouts.forEach((timeout) => clearTimeout(timeout))
      intervals.forEach((interval) => clearInterval(interval))
    }
  }, [])

  return (
    <Card className="relative mx-auto my-0 size-full h-[400px] py-0">
      <div className="flex h-full flex-col">
        <Conversation>
          <ConversationContent>
            {messages.length === 0 && streamingMessageIndex === null ? (
              <ConversationEmptyState
                icon={<Orb className="size-12" />}
                title="Start a conversation"
                description="This is a simulated conversation"
              />
            ) : (
              <>
                {messages.map((message) => (
                  <Message from={message.role} key={message.id}>
                    <MessageContent>
                      {message.parts.map((part, i) => {
                        switch (part.type) {
                          case "text":
                            return (
                              <Response key={`${message.id}-${i}`}>
                                {part.text}
                              </Response>
                            )
                          default:
                            return null
                        }
                      })}
                    </MessageContent>
                    {message.role === "assistant" && (
                      <div className="ring-border size-8 overflow-hidden rounded-full ring-1">
                        <Orb className="h-full w-full" agentState={null} />
                      </div>
                    )}
                  </Message>
                ))}
                {streamingMessageIndex !== null && (
                  <Message
                    from={allMessages[streamingMessageIndex].role}
                    key={`streaming-${streamingMessageIndex}`}
                  >
                    <MessageContent>
                      <Response>{streamingContent || "\u200B"}</Response>
                    </MessageContent>
                    {allMessages[streamingMessageIndex].role ===
                      "assistant" && (
                      <div className="ring-border size-8 overflow-hidden rounded-full ring-1">
                        <Orb className="h-full w-full" agentState="talking" />
                      </div>
                    )}
                  </Message>
                )}
              </>
            )}
          </ConversationContent>
          <ConversationScrollButton />
        </Conversation>
      </div>
    </Card>
  )
}

export default ConversationDemo
```

#### Component Source Code

##### File: `components/ui/conversation.tsx`

```tsx
"use client"

import type { ComponentProps } from "react"
import { useCallback } from "react"
import { ArrowDownIcon } from "lucide-react"
import { StickToBottom, useStickToBottomContext } from "use-stick-to-bottom"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"

export type ConversationProps = ComponentProps<typeof StickToBottom>

export const Conversation = ({ className, ...props }: ConversationProps) => (
  <StickToBottom
    className={cn("relative flex-1 overflow-y-auto", className)}
    initial="smooth"
    resize="smooth"
    role="log"
    {...props}
  />
)

export type ConversationContentProps = ComponentProps<
  typeof StickToBottom.Content
>

export const ConversationContent = ({
  className,
  ...props
}: ConversationContentProps) => (
  <StickToBottom.Content className={cn("p-4", className)} {...props} />
)

export type ConversationEmptyStateProps = Omit<
  ComponentProps<"div">,
  "title"
> & {
  title?: React.ReactNode
  description?: React.ReactNode
  icon?: React.ReactNode
}

export const ConversationEmptyState = ({
  className,
  title = "No messages yet",
  description = "Start a conversation to see messages here",
  icon,
  children,
  ...props
}: ConversationEmptyStateProps) => (
  <div
    className={cn(
      "flex size-full flex-col items-center justify-center gap-3 p-8 text-center",
      className
    )}
    {...props}
  >
    {children ?? (
      <>
        {icon && <div className="text-muted-foreground">{icon}</div>}
        <div className="space-y-1">
          <h3 className="text-sm font-medium">{title}</h3>
          {description && (
            <p className="text-muted-foreground text-sm">{description}</p>
          )}
        </div>
      </>
    )}
  </div>
)

export type ConversationScrollButtonProps = ComponentProps<typeof Button>

export const ConversationScrollButton = ({
  className,
  ...props
}: ConversationScrollButtonProps) => {
  const { isAtBottom, scrollToBottom } = useStickToBottomContext()

  const handleScrollToBottom = useCallback(() => {
    scrollToBottom()
  }, [scrollToBottom])

  return (
    !isAtBottom && (
      <Button
        className={cn(
          "bg-background dark:bg-background absolute bottom-4 left-[50%] translate-x-[-50%] rounded-full shadow-md",
          className
        )}
        onClick={handleScrollToBottom}
        size="icon"
        type="button"
        variant="outline"
        {...props}
      >
        <ArrowDownIcon className="size-4" />
      </Button>
    )
  )
}
```

---

### Live Waveform <a name="live-waveform"></a>

Real-time canvas-based audio waveform visualizer with microphone input and customizable rendering modes.

- **Registry Key**: `live-waveform`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/live-waveform.json
```

#### Demo Code

##### File: `examples/live-waveform-demo.tsx`

```tsx
"use client"

import { useState } from "react"

import { Button } from "@/components/ui/button"
import { LiveWaveform } from "@/components/ui/live-waveform"

export default function LiveWaveformDemo() {
  const [active, setActive] = useState(false)
  const [processing, setProcessing] = useState(false)
  const [mode, setMode] = useState<"static" | "scrolling">("static")

  const handleToggleActive = () => {
    setActive(!active)
    if (!active) {
      setProcessing(false)
    }
  }

  const handleToggleProcessing = () => {
    setProcessing(!processing)
    if (!processing) {
      setActive(false)
    }
  }

  return (
    <div className="bg-card w-full rounded-lg border p-6">
      <div className="mb-4">
        <h3 className="text-lg font-semibold">Live Audio Waveform</h3>
        <p className="text-muted-foreground text-sm">
          Real-time microphone input visualization with audio reactivity
        </p>
      </div>

      <div className="space-y-4">
        <LiveWaveform
          active={active}
          processing={processing}
          height={80}
          barWidth={3}
          barGap={2}
          mode={mode}
          fadeEdges={true}
          barColor="gray"
          historySize={120}
        />

        <div className="flex flex-wrap justify-center gap-2">
          <Button
            size="sm"
            variant={active ? "default" : "outline"}
            onClick={handleToggleActive}
          >
            {active ? "Stop" : "Start"} Listening
          </Button>
          <Button
            size="sm"
            variant={processing ? "default" : "outline"}
            onClick={handleToggleProcessing}
          >
            {processing ? "Stop" : "Start"} Processing
          </Button>
          <Button
            size="sm"
            variant="outline"
            onClick={() => setMode(mode === "static" ? "scrolling" : "static")}
          >
            Mode: {mode === "static" ? "Static" : "Scrolling"}
          </Button>
        </div>
      </div>
    </div>
  )
}
```

#### Props & API Reference

##### `Props` Props

| Prop                  | Type                            | Default    | Description                                    |
| --------------------- | ------------------------------- | ---------- | ---------------------------------------------- |
| active                | `boolean`                       | `false`    | Whether to actively listen to microphone input |
| processing            | `boolean`                       | `false`    | Show processing animation when not active      |
| barWidth              | `number`                        | `3`        | Width of each bar in pixels                    |
| barHeight             | `number`                        | `4`        | Height of each bar in pixels                   |
| barGap                | `number`                        | `1`        | Gap between bars in pixels                     |
| barRadius             | `number`                        | `1.5`      | Border radius of bars                          |
| barColor              | `string`                        | -          | Color of the bars (defaults to text color)     |
| fadeEdges             | `boolean`                       | `true`     | Whether to fade the edges of the waveform      |
| fadeWidth             | `number`                        | `24`       | Width of the fade effect in pixels             |
| height                | `string \| number`              | `64`       | Height of the waveform                         |
| sensitivity           | `number`                        | `1`        | Audio sensitivity multiplier                   |
| smoothingTimeConstant | `number`                        | `0.8`      | Audio analyser smoothing (0-1)                 |
| fftSize               | `number`                        | `256`      | FFT size for audio analysis                    |
| historySize           | `number`                        | `60`       | Number of bars to keep in history (scrolling)  |
| updateRate            | `number`                        | `30`       | Update rate in milliseconds                    |
| mode                  | `"scrolling" \| "static"`       | `"static"` | Visualization mode                             |
| onError               | `(error: Error) => void`        | -          | Error callback                                 |
| onStreamReady         | `(stream: MediaStream) => void` | -          | Callback when stream is ready                  |
| onStreamEnd           | `() => void`                    | -          | Callback when stream ends                      |
| className             | `string`                        | -          | Custom CSS class                               |
| ...props              | `HTMLDivElement`                | -          | All standard div element props                 |

#### Component Source Code

##### File: `components/ui/live-waveform.tsx`

```tsx
"use client"

import { useEffect, useRef, type HTMLAttributes } from "react"

import { cn } from "@/lib/utils"

export type LiveWaveformProps = HTMLAttributes<HTMLDivElement> & {
  active?: boolean
  processing?: boolean
  deviceId?: string
  barWidth?: number
  barHeight?: number
  barGap?: number
  barRadius?: number
  barColor?: string
  fadeEdges?: boolean
  fadeWidth?: number
  height?: string | number
  sensitivity?: number
  smoothingTimeConstant?: number
  fftSize?: number
  historySize?: number
  updateRate?: number
  mode?: "scrolling" | "static"
  onError?: (error: Error) => void
  onStreamReady?: (stream: MediaStream) => void
  onStreamEnd?: () => void
}

export const LiveWaveform = ({
  active = false,
  processing = false,
  deviceId,
  barWidth = 3,
  barGap = 1,
  barRadius = 1.5,
  barColor,
  fadeEdges = true,
  fadeWidth = 24,
  barHeight: baseBarHeight = 4,
  height = 64,
  sensitivity = 1,
  smoothingTimeConstant = 0.8,
  fftSize = 256,
  historySize = 60,
  updateRate = 30,
  mode = "static",
  onError,
  onStreamReady,
  onStreamEnd,
  className,
  ...props
}: LiveWaveformProps) => {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const containerRef = useRef<HTMLDivElement>(null)
  const historyRef = useRef<number[]>([])
  const analyserRef = useRef<AnalyserNode | null>(null)
  const audioContextRef = useRef<AudioContext | null>(null)
  const streamRef = useRef<MediaStream | null>(null)
  const animationRef = useRef<number>(0)
  const lastUpdateRef = useRef<number>(0)
  const processingAnimationRef = useRef<number | null>(null)
  const lastActiveDataRef = useRef<number[]>([])
  const transitionProgressRef = useRef(0)
  const staticBarsRef = useRef<number[]>([])
  const needsRedrawRef = useRef(true)
  const gradientCacheRef = useRef<CanvasGradient | null>(null)
  const lastWidthRef = useRef(0)

  const heightStyle = typeof height === "number" ? `${height}px` : height

  // Handle canvas resizing
  useEffect(() => {
    const canvas = canvasRef.current
    const container = containerRef.current
    if (!canvas || !container) return

    const resizeObserver = new ResizeObserver(() => {
      const rect = container.getBoundingClientRect()
      const dpr = window.devicePixelRatio || 1

      canvas.width = rect.width * dpr
      canvas.height = rect.height * dpr
      canvas.style.width = `${rect.width}px`
      canvas.style.height = `${rect.height}px`

      const ctx = canvas.getContext("2d")
      if (ctx) {
        ctx.scale(dpr, dpr)
      }

      gradientCacheRef.current = null
      lastWidthRef.current = rect.width
      needsRedrawRef.current = true
    })

    resizeObserver.observe(container)
    return () => resizeObserver.disconnect()
  }, [])

  useEffect(() => {
    if (processing && !active) {
      let time = 0
      transitionProgressRef.current = 0

      const animateProcessing = () => {
        time += 0.03
        transitionProgressRef.current = Math.min(
          1,
          transitionProgressRef.current + 0.02
        )

        const processingData = []
        const barCount = Math.floor(
          (containerRef.current?.getBoundingClientRect().width || 200) /
            (barWidth + barGap)
        )

        if (mode === "static") {
          const halfCount = Math.floor(barCount / 2)

          for (let i = 0; i < barCount; i++) {
            const normalizedPosition = (i - halfCount) / halfCount
            const centerWeight = 1 - Math.abs(normalizedPosition) * 0.4

            const wave1 = Math.sin(time * 1.5 + normalizedPosition * 3) * 0.25
            const wave2 = Math.sin(time * 0.8 - normalizedPosition * 2) * 0.2
            const wave3 = Math.cos(time * 2 + normalizedPosition) * 0.15
            const combinedWave = wave1 + wave2 + wave3
            const processingValue = (0.2 + combinedWave) * centerWeight

            let finalValue = processingValue
            if (
              lastActiveDataRef.current.length > 0 &&
              transitionProgressRef.current < 1
            ) {
              const lastDataIndex = Math.min(
                i,
                lastActiveDataRef.current.length - 1
              )
              const lastValue = lastActiveDataRef.current[lastDataIndex] || 0
              finalValue =
                lastValue * (1 - transitionProgressRef.current) +
                processingValue * transitionProgressRef.current
            }

            processingData.push(Math.max(0.05, Math.min(1, finalValue)))
          }
        } else {
          for (let i = 0; i < barCount; i++) {
            const normalizedPosition = (i - barCount / 2) / (barCount / 2)
            const centerWeight = 1 - Math.abs(normalizedPosition) * 0.4

            const wave1 = Math.sin(time * 1.5 + i * 0.15) * 0.25
            const wave2 = Math.sin(time * 0.8 - i * 0.1) * 0.2
            const wave3 = Math.cos(time * 2 + i * 0.05) * 0.15
            const combinedWave = wave1 + wave2 + wave3
            const processingValue = (0.2 + combinedWave) * centerWeight

            let finalValue = processingValue
            if (
              lastActiveDataRef.current.length > 0 &&
              transitionProgressRef.current < 1
            ) {
              const lastDataIndex = Math.floor(
                (i / barCount) * lastActiveDataRef.current.length
              )
              const lastValue = lastActiveDataRef.current[lastDataIndex] || 0
              finalValue =
                lastValue * (1 - transitionProgressRef.current) +
                processingValue * transitionProgressRef.current
            }

            processingData.push(Math.max(0.05, Math.min(1, finalValue)))
          }
        }

        if (mode === "static") {
          staticBarsRef.current = processingData
        } else {
          historyRef.current = processingData
        }

        needsRedrawRef.current = true
        processingAnimationRef.current =
          requestAnimationFrame(animateProcessing)
      }

      animateProcessing()

      return () => {
        if (processingAnimationRef.current) {
          cancelAnimationFrame(processingAnimationRef.current)
        }
      }
    } else if (!active && !processing) {
      const hasData =
        mode === "static"
          ? staticBarsRef.current.length > 0
          : historyRef.current.length > 0

      if (hasData) {
        let fadeProgress = 0
        const fadeToIdle = () => {
          fadeProgress += 0.03
          if (fadeProgress < 1) {
            if (mode === "static") {
              staticBarsRef.current = staticBarsRef.current.map(
                (value) => value * (1 - fadeProgress)
              )
            } else {
              historyRef.current = historyRef.current.map(
                (value) => value * (1 - fadeProgress)
              )
            }
            needsRedrawRef.current = true
            requestAnimationFrame(fadeToIdle)
          } else {
            if (mode === "static") {
              staticBarsRef.current = []
            } else {
              historyRef.current = []
            }
          }
        }
        fadeToIdle()
      }
    }
  }, [processing, active, barWidth, barGap, mode])

  // Handle microphone setup and teardown
  useEffect(() => {
    if (!active) {
      if (streamRef.current) {
        streamRef.current.getTracks().forEach((track) => track.stop())
        streamRef.current = null
        onStreamEnd?.()
      }
      if (
        audioContextRef.current &&
        audioContextRef.current.state !== "closed"
      ) {
        audioContextRef.current.close()
        audioContextRef.current = null
      }
      if (animationRef.current) {
        cancelAnimationFrame(animationRef.current)
        animationRef.current = 0
      }
      return
    }

    const setupMicrophone = async () => {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({
          audio: deviceId
            ? {
                deviceId: { exact: deviceId },
                echoCancellation: true,
                noiseSuppression: true,
                autoGainControl: true,
              }
            : {
                echoCancellation: true,
                noiseSuppression: true,
                autoGainControl: true,
              },
        })
        streamRef.current = stream
        onStreamReady?.(stream)

        const AudioContextConstructor =
          window.AudioContext ||
          (window as unknown as { webkitAudioContext: typeof AudioContext })
            .webkitAudioContext
        const audioContext = new AudioContextConstructor()
        const analyser = audioContext.createAnalyser()
        analyser.fftSize = fftSize
        analyser.smoothingTimeConstant = smoothingTimeConstant

        const source = audioContext.createMediaStreamSource(stream)
        source.connect(analyser)

        audioContextRef.current = audioContext
        analyserRef.current = analyser

        // Clear history when starting
        historyRef.current = []
      } catch (error) {
        onError?.(error as Error)
      }
    }

    setupMicrophone()

    return () => {
      if (streamRef.current) {
        streamRef.current.getTracks().forEach((track) => track.stop())
        streamRef.current = null
        onStreamEnd?.()
      }
      if (
        audioContextRef.current &&
        audioContextRef.current.state !== "closed"
      ) {
        audioContextRef.current.close()
        audioContextRef.current = null
      }
      if (animationRef.current) {
        cancelAnimationFrame(animationRef.current)
        animationRef.current = 0
      }
    }
  }, [
    active,
    deviceId,
    fftSize,
    smoothingTimeConstant,
    onError,
    onStreamReady,
    onStreamEnd,
  ])

  // Animation loop
  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return

    const ctx = canvas.getContext("2d")
    if (!ctx) return

    let rafId: number

    const animate = (currentTime: number) => {
      // Render waveform
      const rect = canvas.getBoundingClientRect()

      // Update audio data if active
      if (active && currentTime - lastUpdateRef.current > updateRate) {
        lastUpdateRef.current = currentTime

        if (analyserRef.current) {
          const dataArray = new Uint8Array(
            analyserRef.current.frequencyBinCount
          )
          analyserRef.current.getByteFrequencyData(dataArray)

          if (mode === "static") {
            // For static mode, update bars in place
            const startFreq = Math.floor(dataArray.length * 0.05)
            const endFreq = Math.floor(dataArray.length * 0.4)
            const relevantData = dataArray.slice(startFreq, endFreq)

            const barCount = Math.floor(rect.width / (barWidth + barGap))
            const halfCount = Math.floor(barCount / 2)
            const newBars: number[] = []

            // Mirror the data for symmetric display
            for (let i = halfCount - 1; i >= 0; i--) {
              const dataIndex = Math.floor(
                (i / halfCount) * relevantData.length
              )
              const value = Math.min(
                1,
                (relevantData[dataIndex] / 255) * sensitivity
              )
              newBars.push(Math.max(0.05, value))
            }

            for (let i = 0; i < halfCount; i++) {
              const dataIndex = Math.floor(
                (i / halfCount) * relevantData.length
              )
              const value = Math.min(
                1,
                (relevantData[dataIndex] / 255) * sensitivity
              )
              newBars.push(Math.max(0.05, value))
            }

            staticBarsRef.current = newBars
            lastActiveDataRef.current = newBars
          } else {
            // Scrolling mode - original behavior
            let sum = 0
            const startFreq = Math.floor(dataArray.length * 0.05)
            const endFreq = Math.floor(dataArray.length * 0.4)
            const relevantData = dataArray.slice(startFreq, endFreq)

            for (let i = 0; i < relevantData.length; i++) {
              sum += relevantData[i]
            }
            const average = (sum / relevantData.length / 255) * sensitivity

            // Add to history
            historyRef.current.push(Math.min(1, Math.max(0.05, average)))
            lastActiveDataRef.current = [...historyRef.current]

            // Maintain history size
            if (historyRef.current.length > historySize) {
              historyRef.current.shift()
            }
          }
          needsRedrawRef.current = true
        }
      }

      // Only redraw if needed
      if (!needsRedrawRef.current && !active) {
        rafId = requestAnimationFrame(animate)
        return
      }

      needsRedrawRef.current = active
      ctx.clearRect(0, 0, rect.width, rect.height)

      const computedBarColor =
        barColor ||
        (() => {
          const style = getComputedStyle(canvas)
          // Try to get the computed color value directly
          const color = style.color
          return color || "#000"
        })()

      const step = barWidth + barGap
      const barCount = Math.floor(rect.width / step)
      const centerY = rect.height / 2

      // Draw bars based on mode
      if (mode === "static") {
        // Static mode - bars in fixed positions
        const dataToRender = processing
          ? staticBarsRef.current
          : active
            ? staticBarsRef.current
            : staticBarsRef.current.length > 0
              ? staticBarsRef.current
              : []

        for (let i = 0; i < barCount && i < dataToRender.length; i++) {
          const value = dataToRender[i] || 0.1
          const x = i * step
          const barHeight = Math.max(baseBarHeight, value * rect.height * 0.8)
          const y = centerY - barHeight / 2

          ctx.fillStyle = computedBarColor
          ctx.globalAlpha = 0.4 + value * 0.6

          if (barRadius > 0) {
            ctx.beginPath()
            ctx.roundRect(x, y, barWidth, barHeight, barRadius)
            ctx.fill()
          } else {
            ctx.fillRect(x, y, barWidth, barHeight)
          }
        }
      } else {
        // Scrolling mode - original behavior
        for (let i = 0; i < barCount && i < historyRef.current.length; i++) {
          const dataIndex = historyRef.current.length - 1 - i
          const value = historyRef.current[dataIndex] || 0.1
          const x = rect.width - (i + 1) * step
          const barHeight = Math.max(baseBarHeight, value * rect.height * 0.8)
          const y = centerY - barHeight / 2

          ctx.fillStyle = computedBarColor
          ctx.globalAlpha = 0.4 + value * 0.6

          if (barRadius > 0) {
            ctx.beginPath()
            ctx.roundRect(x, y, barWidth, barHeight, barRadius)
            ctx.fill()
          } else {
            ctx.fillRect(x, y, barWidth, barHeight)
          }
        }
      }

      // Apply edge fading
      if (fadeEdges && fadeWidth > 0 && rect.width > 0) {
        // Cache gradient if width hasn't changed
        if (!gradientCacheRef.current || lastWidthRef.current !== rect.width) {
          const gradient = ctx.createLinearGradient(0, 0, rect.width, 0)
          const fadePercent = Math.min(0.3, fadeWidth / rect.width)

          // destination-out: removes destination where source alpha is high
          // We want: fade edges out, keep center solid
          // Left edge: start opaque (1) = remove, fade to transparent (0) = keep
          gradient.addColorStop(0, "rgba(255,255,255,1)")
          gradient.addColorStop(fadePercent, "rgba(255,255,255,0)")
          // Center stays transparent = keep everything
          gradient.addColorStop(1 - fadePercent, "rgba(255,255,255,0)")
          // Right edge: fade from transparent (0) = keep to opaque (1) = remove
          gradient.addColorStop(1, "rgba(255,255,255,1)")

          gradientCacheRef.current = gradient
          lastWidthRef.current = rect.width
        }

        ctx.globalCompositeOperation = "destination-out"
        ctx.fillStyle = gradientCacheRef.current
        ctx.fillRect(0, 0, rect.width, rect.height)
        ctx.globalCompositeOperation = "source-over"
      }

      ctx.globalAlpha = 1

      rafId = requestAnimationFrame(animate)
    }

    rafId = requestAnimationFrame(animate)

    return () => {
      if (rafId) {
        cancelAnimationFrame(rafId)
      }
    }
  }, [
    active,
    processing,
    sensitivity,
    updateRate,
    historySize,
    barWidth,
    baseBarHeight,
    barGap,
    barRadius,
    barColor,
    fadeEdges,
    fadeWidth,
    mode,
  ])

  return (
    <div
      className={cn("relative h-full w-full", className)}
      ref={containerRef}
      style={{ height: heightStyle }}
      aria-label={
        active
          ? "Live audio waveform"
          : processing
            ? "Processing audio"
            : "Audio waveform idle"
      }
      role="img"
      {...props}
    >
      {!active && !processing && (
        <div className="border-muted-foreground/20 absolute top-1/2 right-0 left-0 -translate-y-1/2 border-t-2 border-dotted" />
      )}
      <canvas
        className="block h-full w-full"
        ref={canvasRef}
        aria-hidden="true"
      />
    </div>
  )
}
```

---

### Matrix <a name="matrix"></a>

A retro dot-matrix display component with circular cells and smooth animations. Perfect for retro displays, indicators, and audio visualizations.

- **Registry Key**: `matrix`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/matrix.json
```

#### How to Use

##### Example 1

```tsx
<Matrix rows={7} cols={7} frames={wave} fps={20} />
```

##### Example 2

```tsx
type Frame = number[][] // [row][col] brightness 0..1
```

##### Example 3

```tsx
const smiley: Frame = [
  [0, 1, 1, 1, 0],
  [1, 0, 0, 0, 1],
  [1, 1, 0, 1, 1],
  [1, 0, 0, 0, 1],
  [0, 1, 1, 1, 0],
]
```

##### Example 4

```tsx
import { digits, Matrix } from "@/components/ui/matrix"

export default () => <Matrix rows={7} cols={5} pattern={digits[5]} />
```

##### Example 5

```tsx
import { loader, Matrix } from "@/components/ui/matrix"

export default () => <Matrix rows={7} cols={7} frames={loader} fps={12} />
```

##### Example 6

```tsx
import { Matrix, pulse } from "@/components/ui/matrix"

export default () => <Matrix rows={7} cols={7} frames={pulse} fps={16} />
```

##### Example 7

```tsx
import { Matrix, wave } from "@/components/ui/matrix"

export default () => <Matrix rows={7} cols={7} frames={wave} fps={20} />
```

##### Example 8

```tsx
import { Matrix, snake } from "@/components/ui/matrix"

export default () => <Matrix rows={7} cols={7} frames={snake} fps={15} />
```

##### Example 9

```tsx
import { chevronLeft, Matrix } from "@/components/ui/matrix"

export default () => <Matrix rows={5} cols={5} pattern={chevronLeft} />
```

##### Example 10

```tsx
import { Matrix, vu } from "@/components/ui/matrix"

export default () => {
  const levels = [0.3, 0.6, 0.9, 0.7, 0.5]
  return <Matrix rows={7} cols={5} pattern={vu(5, levels)} />
}
```

##### Example 11

```tsx
<Matrix
  palette={{
    on: "currentColor", // Active cells (default - inherits text color)
    off: "var(--muted-foreground)", // Inactive cells (default - muted but visible)
  }}
/>
```

##### Example 12

```tsx
palette={{
  on: "hsl(142 76% 36%)",
  off: "hsl(142 76% 10%)",
}}
```

##### Example 13

```tsx
palette={{
  on: "hsl(38 92% 50%)",
  off: "hsl(38 92% 15%)",
}}
```

##### Example 14

```tsx
palette={{
  on: "hsl(200 98% 39%)",
  off: "hsl(200 98% 12%)",
}}
```

#### Demo Code

##### File: `examples/matrix-demo.tsx`

```tsx
"use client"

import { useEffect, useState } from "react"

import {
  Matrix,
  pulse,
  snake,
  wave,
  type Frame,
} from "@/components/ui/matrix"

const Example = () => {
  const [mode, setMode] = useState<
    "individual" | "focus" | "expand" | "unified" | "collapse" | "burst"
  >("individual")
  const [unifiedFrame, setUnifiedFrame] = useState(0)
  const [expandProgress, setExpandProgress] = useState(0)
  const [collapseProgress, setCollapseProgress] = useState(0)

  useEffect(() => {
    let timeout: NodeJS.Timeout

    if (mode === "individual") {
      timeout = setTimeout(() => setMode("focus"), 4000)
    } else if (mode === "focus") {
      timeout = setTimeout(() => setMode("expand"), 2000)
    } else if (mode === "expand") {
      timeout = setTimeout(() => setMode("unified"), 2500)
    } else if (mode === "unified") {
      timeout = setTimeout(() => setMode("collapse"), 4000)
    } else if (mode === "collapse") {
      timeout = setTimeout(() => setMode("burst"), 2500)
    } else if (mode === "burst") {
      timeout = setTimeout(() => setMode("individual"), 800)
    }

    return () => clearTimeout(timeout)
  }, [mode])

  useEffect(() => {
    if (mode !== "unified") return

    let frame = 0
    const animate = setInterval(() => {
      frame += 1
      setUnifiedFrame(frame)
    }, 50)

    return () => clearInterval(animate)
  }, [mode])

  useEffect(() => {
    if (mode !== "expand") {
      setExpandProgress(0)
      return
    }

    let start = 0
    const duration = 2500
    let animationFrame: number

    const animate = (timestamp: number) => {
      if (start === 0) start = timestamp
      const elapsed = timestamp - start
      const progress = Math.min(elapsed / duration, 1)

      setExpandProgress(progress)

      if (progress < 1) {
        animationFrame = requestAnimationFrame(animate)
      }
    }

    animationFrame = requestAnimationFrame(animate)
    return () => cancelAnimationFrame(animationFrame)
  }, [mode])

  useEffect(() => {
    if (mode !== "collapse") {
      setCollapseProgress(0)
      return
    }

    let start = 0
    const duration = 2500
    let animationFrame: number

    const animate = (timestamp: number) => {
      if (start === 0) start = timestamp
      const elapsed = timestamp - start
      const progress = Math.min(elapsed / duration, 1)

      setCollapseProgress(progress)

      if (progress < 1) {
        animationFrame = requestAnimationFrame(animate)
      }
    }

    animationFrame = requestAnimationFrame(animate)
    return () => cancelAnimationFrame(animationFrame)
  }, [mode])

  const configurations = [
    { animation: pulse, fps: 16 },
    { animation: wave, fps: 20 },
    { animation: spinner, fps: 10 },
    { animation: snake, fps: 15 },
    { animation: elevenLogo, fps: 12 },
    { animation: sandTimer, fps: 12 },
    { animation: corners, fps: 10 },
    { animation: sweep, fps: 14 },
    { animation: expand, fps: 12 },
  ]

  const unifiedPatterns =
    mode === "unified" ? createUnifiedPattern(unifiedFrame) : []

  const expandedPatterns =
    mode === "expand" ? createExpandedLogo(expandProgress) : []
  const collapsePatterns =
    mode === "collapse" ? createCollapseEffect(collapseProgress) : []

  const getPatternForMatrix = (index: number) => {
    if (mode === "individual") {
      return undefined
    } else if (mode === "focus") {
      if (index === 4) {
        const frame: Frame = Array(7)
          .fill(0)
          .map(() => Array(7).fill(0))
        for (let r = 1; r <= 5; r++) {
          frame[r][2] = 1
          frame[r][4] = 1
        }
        return frame
      }
      return Array(7)
        .fill(0)
        .map(() => Array(7).fill(0))
    } else if (mode === "expand") {
      return expandedPatterns[index]
    } else if (mode === "unified") {
      return unifiedPatterns[index]
    } else if (mode === "collapse") {
      return collapsePatterns[index]
    } else if (mode === "burst") {
      return undefined
    }
  }

  const getFramesForMatrix = (index: number) => {
    if (mode === "individual") {
      return configurations[index].animation
    } else if (mode === "burst") {
      return burst
    }
    return undefined
  }

  const getFps = (index: number) => {
    if (mode === "burst") return 30
    return configurations[index].fps
  }

  return (
    <div className="flex min-h-[600px] w-full flex-col items-center justify-center p-8">
      <div
        className="grid gap-1.5 transition-all duration-1000"
        style={{
          gridTemplateColumns: "repeat(3, 1fr)",
          gridTemplateRows: "repeat(3, 1fr)",
        }}
      >
        {configurations.map((config, index) => (
          <div
            key={index}
            className="flex items-center justify-center transition-all duration-1000"
          >
            <Matrix
              rows={7}
              cols={7}
              frames={getFramesForMatrix(index)}
              pattern={getPatternForMatrix(index)}
              fps={getFps(index)}
              size={10}
              gap={2}
              ariaLabel={`Matrix ${index + 1}`}
            />
          </div>
        ))}
      </div>
    </div>
  )
}
function createUnifiedPattern(frameIndex: number): Frame[] {
  const totalRows = 21
  const totalCols = 21
  const pattern: number[][] = []

  for (let row = 0; row < totalRows; row++) {
    pattern[row] = []
    for (let col = 0; col < totalCols; col++) {
      const centerX = totalCols / 2
      const centerY = totalRows / 2
      const distance = Math.sqrt(
        Math.pow(col - centerX, 2) + Math.pow(row - centerY, 2)
      )
      const wave = Math.sin(distance * 0.5 - frameIndex * 0.2)
      const value = (wave + 1) / 2
      pattern[row][col] = value
    }
  }

  const matrices: Frame[] = []
  for (let matrixRow = 0; matrixRow < 3; matrixRow++) {
    for (let matrixCol = 0; matrixCol < 3; matrixCol++) {
      const matrixFrame: Frame = []
      for (let row = 0; row < 7; row++) {
        matrixFrame[row] = []
        for (let col = 0; col < 7; col++) {
          const globalRow = matrixRow * 7 + row
          const globalCol = matrixCol * 7 + col
          matrixFrame[row][col] = pattern[globalRow][globalCol]
        }
      }
      matrices.push(matrixFrame)
    }
  }

  return matrices
}

const elevenLogo: Frame[] = (() => {
  const frames: Frame[] = []
  const totalFrames = 30

  for (let f = 0; f < totalFrames; f++) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))

    const phase = (f / totalFrames) * Math.PI * 2
    const intensity = ((Math.sin(phase) + 1) / 2) * 0.3 + 0.7

    for (let r = 1; r <= 5; r++) {
      frame[r][2] = intensity
      frame[r][4] = intensity
    }

    frames.push(frame)
  }
  return frames
})()

const sandTimer: Frame[] = (() => {
  const frames: Frame[] = []
  const totalFrames = 60

  for (let f = 0; f < totalFrames; f++) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))

    frame[0][2] = 1
    frame[0][3] = 1
    frame[0][4] = 1
    frame[1][2] = 1
    frame[1][4] = 1
    frame[5][2] = 1
    frame[5][4] = 1
    frame[6][2] = 1
    frame[6][3] = 1
    frame[6][4] = 1

    const progress = f / totalFrames

    const topSand = Math.floor((1 - progress) * 8)
    for (let i = 0; i < topSand; i++) {
      if (i < 3) frame[1][3] = 1
      if (i >= 3) frame[2][3] = 1
    }

    const bottomSand = Math.floor(progress * 8)
    for (let i = 0; i < bottomSand; i++) {
      if (i < 3) frame[5][3] = 1
      if (i >= 3 && i < 6) frame[4][3] = 1
      if (i >= 6) frame[3][3] = 0.5
    }

    frames.push(frame)
  }
  return frames
})()

const spinner: Frame[] = (() => {
  const frames: Frame[] = []
  const segments = 8

  for (let f = 0; f < segments; f++) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))

    const positions = [
      [1, 3],
      [1, 4],
      [2, 5],
      [3, 5],
      [4, 5],
      [5, 4],
      [5, 3],
      [5, 2],
      [4, 1],
      [3, 1],
      [2, 1],
      [1, 2],
    ]

    for (let i = 0; i < 3; i++) {
      const idx = (f + i) % positions.length
      const [r, c] = positions[idx]
      frame[r][c] = 1 - i * 0.3
    }

    frames.push(frame)
  }
  return frames
})()

const corners: Frame[] = (() => {
  const frames: Frame[] = []
  for (let i = 0; i < 16; i++) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))
    const progress = i / 16

    for (let r = 0; r < 7; r++) {
      for (let c = 0; c < 7; c++) {
        const distFromCorner = Math.min(
          Math.sqrt(r * r + c * c),
          Math.sqrt(r * r + (6 - c) * (6 - c)),
          Math.sqrt((6 - r) * (6 - r) + c * c),
          Math.sqrt((6 - r) * (6 - r) + (6 - c) * (6 - c))
        )
        const threshold = progress * 8
        if (distFromCorner <= threshold) {
          frame[r][c] = Math.max(0, 1 - Math.abs(distFromCorner - threshold))
        }
      }
    }
    frames.push(frame)
  }
  return frames
})()

const sweep: Frame[] = (() => {
  const frames: Frame[] = []
  for (let i = 0; i < 14; i++) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))
    for (let r = 0; r < 7; r++) {
      for (let c = 0; c < 7; c++) {
        if (r + c === i) {
          frame[r][c] = 1
        } else if (r + c === i - 1) {
          frame[r][c] = 0.5
        } else if (r + c === i + 1) {
          frame[r][c] = 0.5
        }
      }
    }
    frames.push(frame)
  }
  return frames
})()

const expand: Frame[] = (() => {
  const frames: Frame[] = []
  for (let i = 0; i <= 6; i++) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))
    for (let r = 3 - i; r <= 3 + i; r++) {
      for (let c = 3 - i; c <= 3 + i; c++) {
        if (r >= 0 && r < 7 && c >= 0 && c < 7) {
          if (r === 3 - i || r === 3 + i || c === 3 - i || c === 3 + i) {
            frame[r][c] = 1
          }
        }
      }
    }
    frames.push(frame)
  }
  for (let i = 5; i >= 0; i--) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))
    for (let r = 3 - i; r <= 3 + i; r++) {
      for (let c = 3 - i; c <= 3 + i; c++) {
        if (r >= 0 && r < 7 && c >= 0 && c < 7) {
          if (r === 3 - i || r === 3 + i || c === 3 - i || c === 3 + i) {
            frame[r][c] = 1
          }
        }
      }
    }
    frames.push(frame)
  }
  return frames
})()

const burst: Frame[] = (() => {
  const frames: Frame[] = []
  for (let f = 0; f < 8; f++) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))

    const intensity = f < 4 ? f / 3 : (7 - f) / 3

    if (f < 6) {
      for (let r = 0; r < 7; r++) {
        for (let c = 0; c < 7; c++) {
          const distance = Math.sqrt(Math.pow(r - 3, 2) + Math.pow(c - 3, 2))
          if (Math.abs(distance - f * 0.8) < 1.2) {
            frame[r][c] = intensity
          }
        }
      }
    }

    frames.push(frame)
  }
  return frames
})()

function createExpandedLogo(progress: number): Frame[] {
  const matrices: Frame[] = []

  for (let matrixIdx = 0; matrixIdx < 9; matrixIdx++) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))
    matrices.push(frame)
  }

  const easeProgress =
    progress < 0.5
      ? 2 * progress * progress
      : 1 - Math.pow(-2 * progress + 2, 2) / 2

  if (easeProgress < 0.3) {
    const centerMatrix = matrices[4]
    for (let r = 1; r <= 5; r++) {
      centerMatrix[r][2] = 1
      centerMatrix[r][4] = 1
    }
    return matrices
  }

  const expandProgress = (easeProgress - 0.3) / 0.7

  for (let globalRow = 0; globalRow < 21; globalRow++) {
    for (let globalCol = 0; globalCol < 21; globalCol++) {
      const matrixRow = Math.floor(globalRow / 7)
      const matrixCol = Math.floor(globalCol / 7)
      const matrixIdx = matrixRow * 3 + matrixCol
      const localRow = globalRow % 7
      const localCol = globalCol % 7

      const leftBarStart = Math.floor(9 + (5 - 9) * expandProgress)
      const leftBarEnd = Math.floor(9 + (7 - 9) * expandProgress)
      const rightBarStart = Math.floor(11 + (13 - 11) * expandProgress)
      const rightBarEnd = Math.floor(11 + (15 - 11) * expandProgress)

      const isLeftBar = globalCol >= leftBarStart && globalCol <= leftBarEnd
      const isRightBar = globalCol >= rightBarStart && globalCol <= rightBarEnd
      const inVerticalRange = globalRow >= 4 && globalRow <= 16

      if ((isLeftBar || isRightBar) && inVerticalRange) {
        matrices[matrixIdx][localRow][localCol] = 1
      }
    }
  }

  return matrices
}

function createCollapseEffect(progress: number): Frame[] {
  const matrices: Frame[] = []

  for (let matrixIdx = 0; matrixIdx < 9; matrixIdx++) {
    const frame: Frame = Array(7)
      .fill(0)
      .map(() => Array(7).fill(0))
    matrices.push(frame)
  }

  const easeProgress =
    progress < 0.5
      ? 2 * progress * progress
      : 1 - Math.pow(-2 * progress + 2, 2) / 2

  if (easeProgress < 0.4) {
    const collapseProgress = easeProgress / 0.4

    for (let globalRow = 0; globalRow < 21; globalRow++) {
      for (let globalCol = 0; globalCol < 21; globalCol++) {
        const matrixRow = Math.floor(globalRow / 7)
        const matrixCol = Math.floor(globalCol / 7)
        const matrixIdx = matrixRow * 3 + matrixCol
        const localRow = globalRow % 7
        const localCol = globalCol % 7

        const leftBarStart = Math.floor(5 + (9 - 5) * collapseProgress)
        const leftBarEnd = Math.floor(7 + (9 - 7) * collapseProgress)
        const rightBarStart = Math.floor(13 + (11 - 13) * collapseProgress)
        const rightBarEnd = Math.floor(15 + (11 - 15) * collapseProgress)

        const isLeftBar = globalCol >= leftBarStart && globalCol <= leftBarEnd
        const isRightBar =
          globalCol >= rightBarStart && globalCol <= rightBarEnd
        const inVerticalRange = globalRow >= 4 && globalRow <= 16

        if ((isLeftBar || isRightBar) && inVerticalRange) {
          matrices[matrixIdx][localRow][localCol] = 1
        }
      }
    }
  } else {
    const centerMatrix = matrices[4]
    const fadeProgress = (easeProgress - 0.4) / 0.6
    const brightness = 1 - fadeProgress

    for (let r = 1; r <= 5; r++) {
      centerMatrix[r][2] = brightness
      centerMatrix[r][4] = brightness
    }
  }

  return matrices
}

export default Example
```

#### Component Source Code

##### File: `components/ui/matrix.tsx`

```tsx
"use client"

import * as React from "react"
import { useEffect, useMemo, useRef, useState } from "react"

import { cn } from "@/lib/utils"

export type Frame = number[][]
type MatrixMode = "default" | "vu"

interface CellPosition {
  x: number
  y: number
}

interface MatrixProps extends React.HTMLAttributes<HTMLDivElement> {
  rows: number
  cols: number
  pattern?: Frame
  frames?: Frame[]
  fps?: number
  autoplay?: boolean
  loop?: boolean
  size?: number
  gap?: number
  palette?: {
    on: string
    off: string
  }
  brightness?: number
  ariaLabel?: string
  onFrame?: (index: number) => void
  mode?: MatrixMode
  levels?: number[]
}

function clamp(value: number): number {
  return Math.max(0, Math.min(1, value))
}

function ensureFrameSize(frame: Frame, rows: number, cols: number): Frame {
  const result: Frame = []
  for (let r = 0; r < rows; r++) {
    const row = frame[r] || []
    result.push([])
    for (let c = 0; c < cols; c++) {
      result[r][c] = row[c] ?? 0
    }
  }
  return result
}

function useAnimation(
  frames: Frame[] | undefined,
  options: {
    fps: number
    autoplay: boolean
    loop: boolean
    onFrame?: (index: number) => void
  }
): { frameIndex: number; isPlaying: boolean } {
  const [frameIndex, setFrameIndex] = useState(0)
  const [isPlaying, setIsPlaying] = useState(options.autoplay)
  const frameIdRef = useRef<number | undefined>(undefined)
  const lastTimeRef = useRef<number>(0)
  const accumulatorRef = useRef<number>(0)

  useEffect(() => {
    if (!frames || frames.length === 0 || !isPlaying) {
      return
    }

    const frameInterval = 1000 / options.fps

    const animate = (currentTime: number) => {
      if (lastTimeRef.current === 0) {
        lastTimeRef.current = currentTime
      }

      const deltaTime = currentTime - lastTimeRef.current
      lastTimeRef.current = currentTime
      accumulatorRef.current += deltaTime

      if (accumulatorRef.current >= frameInterval) {
        accumulatorRef.current -= frameInterval

        setFrameIndex((prev) => {
          const next = prev + 1
          if (next >= frames.length) {
            if (options.loop) {
              options.onFrame?.(0)
              return 0
            } else {
              setIsPlaying(false)
              return prev
            }
          }
          options.onFrame?.(next)
          return next
        })
      }

      frameIdRef.current = requestAnimationFrame(animate)
    }

    frameIdRef.current = requestAnimationFrame(animate)

    return () => {
      if (frameIdRef.current) {
        cancelAnimationFrame(frameIdRef.current)
      }
    }
  }, [frames, isPlaying, options.fps, options.loop, options.onFrame])

  useEffect(() => {
    setFrameIndex(0)
    setIsPlaying(options.autoplay)
    lastTimeRef.current = 0
    accumulatorRef.current = 0
  }, [frames, options.autoplay])

  return { frameIndex, isPlaying }
}

function emptyFrame(rows: number, cols: number): Frame {
  return Array.from({ length: rows }, () => Array(cols).fill(0))
}

function setPixel(frame: Frame, row: number, col: number, value: number): void {
  if (row >= 0 && row < frame.length && col >= 0 && col < frame[0].length) {
    frame[row][col] = value
  }
}

export const digits: Frame[] = [
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 0, 1, 0, 0],
    [0, 1, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 1, 0],
    [0, 0, 1, 0, 0],
    [0, 1, 0, 0, 0],
    [1, 1, 1, 1, 1],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [0, 0, 0, 0, 1],
    [0, 0, 1, 1, 0],
    [0, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 0, 0, 1, 0],
    [0, 0, 1, 1, 0],
    [0, 1, 0, 1, 0],
    [1, 0, 0, 1, 0],
    [1, 1, 1, 1, 1],
    [0, 0, 0, 1, 0],
    [0, 0, 0, 1, 0],
  ],
  [
    [1, 1, 1, 1, 1],
    [1, 0, 0, 0, 0],
    [1, 1, 1, 1, 0],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 0],
    [1, 0, 0, 0, 0],
    [1, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [1, 1, 1, 1, 1],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 1, 0],
    [0, 0, 1, 0, 0],
    [0, 1, 0, 0, 0],
    [0, 1, 0, 0, 0],
    [0, 1, 0, 0, 0],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
  [
    [0, 1, 1, 1, 0],
    [1, 0, 0, 0, 1],
    [1, 0, 0, 0, 1],
    [0, 1, 1, 1, 1],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 0, 1],
    [0, 1, 1, 1, 0],
  ],
]

export const chevronLeft: Frame = [
  [0, 0, 0, 1, 0],
  [0, 0, 1, 0, 0],
  [0, 1, 0, 0, 0],
  [0, 0, 1, 0, 0],
  [0, 0, 0, 1, 0],
]

export const chevronRight: Frame = [
  [0, 1, 0, 0, 0],
  [0, 0, 1, 0, 0],
  [0, 0, 0, 1, 0],
  [0, 0, 1, 0, 0],
  [0, 1, 0, 0, 0],
]

export const loader: Frame[] = (() => {
  const frames: Frame[] = []
  const size = 7
  const center = 3
  const radius = 2.5

  for (let frame = 0; frame < 12; frame++) {
    const f = emptyFrame(size, size)
    for (let i = 0; i < 8; i++) {
      const angle = (frame / 12) * Math.PI * 2 + (i / 8) * Math.PI * 2
      const x = Math.round(center + Math.cos(angle) * radius)
      const y = Math.round(center + Math.sin(angle) * radius)
      const brightness = 1 - i / 10
      setPixel(f, y, x, Math.max(0.2, brightness))
    }
    frames.push(f)
  }

  return frames
})()

export const pulse: Frame[] = (() => {
  const frames: Frame[] = []
  const size = 7
  const center = 3

  for (let frame = 0; frame < 16; frame++) {
    const f = emptyFrame(size, size)
    const phase = (frame / 16) * Math.PI * 2
    const intensity = (Math.sin(phase) + 1) / 2

    setPixel(f, center, center, 1)

    const radius = Math.floor((1 - intensity) * 3) + 1
    for (let dy = -radius; dy <= radius; dy++) {
      for (let dx = -radius; dx <= radius; dx++) {
        const dist = Math.sqrt(dx * dx + dy * dy)
        if (Math.abs(dist - radius) < 0.7) {
          setPixel(f, center + dy, center + dx, intensity * 0.6)
        }
      }
    }

    frames.push(f)
  }

  return frames
})()

export function vu(columns: number, levels: number[]): Frame {
  const rows = 7
  const frame = emptyFrame(rows, columns)

  for (let col = 0; col < Math.min(columns, levels.length); col++) {
    const level = Math.max(0, Math.min(1, levels[col]))
    const height = Math.floor(level * rows)

    for (let row = 0; row < rows; row++) {
      const rowFromBottom = rows - 1 - row
      if (rowFromBottom < height) {
        let brightness = 1
        if (row < rows * 0.3) {
          brightness = 1
        } else if (row < rows * 0.6) {
          brightness = 0.8
        } else {
          brightness = 0.6
        }
        frame[row][col] = brightness
      }
    }
  }

  return frame
}

export const wave: Frame[] = (() => {
  const frames: Frame[] = []
  const rows = 7
  const cols = 7

  for (let frame = 0; frame < 24; frame++) {
    const f = emptyFrame(rows, cols)
    const phase = (frame / 24) * Math.PI * 2

    for (let col = 0; col < cols; col++) {
      const colPhase = (col / cols) * Math.PI * 2
      const height = Math.sin(phase + colPhase) * 2.5 + 3.5
      const row = Math.floor(height)

      if (row >= 0 && row < rows) {
        setPixel(f, row, col, 1)
        const frac = height - row
        if (row > 0) setPixel(f, row - 1, col, 1 - frac)
        if (row < rows - 1) setPixel(f, row + 1, col, frac)
      }
    }

    frames.push(f)
  }

  return frames
})()

export const snake: Frame[] = (() => {
  const frames: Frame[] = []
  const rows = 7
  const cols = 7
  const path: Array<[number, number]> = []

  let x = 0
  let y = 0
  let dx = 1
  let dy = 0

  const visited = new Set<string>()
  while (path.length < rows * cols) {
    path.push([y, x])
    visited.add(`${y},${x}`)

    const nextX = x + dx
    const nextY = y + dy

    if (
      nextX >= 0 &&
      nextX < cols &&
      nextY >= 0 &&
      nextY < rows &&
      !visited.has(`${nextY},${nextX}`)
    ) {
      x = nextX
      y = nextY
    } else {
      const newDx = -dy
      const newDy = dx
      dx = newDx
      dy = newDy

      const nextX = x + dx
      const nextY = y + dy

      if (
        nextX >= 0 &&
        nextX < cols &&
        nextY >= 0 &&
        nextY < rows &&
        !visited.has(`${nextY},${nextX}`)
      ) {
        x = nextX
        y = nextY
      } else {
        break
      }
    }
  }

  const snakeLength = 5
  for (let frame = 0; frame < path.length; frame++) {
    const f = emptyFrame(rows, cols)

    for (let i = 0; i < snakeLength; i++) {
      const idx = frame - i
      if (idx >= 0 && idx < path.length) {
        const [y, x] = path[idx]
        const brightness = 1 - i / snakeLength
        setPixel(f, y, x, brightness)
      }
    }

    frames.push(f)
  }

  return frames
})()

export const Matrix = React.forwardRef<HTMLDivElement, MatrixProps>(
  (
    {
      rows,
      cols,
      pattern,
      frames,
      fps = 12,
      autoplay = true,
      loop = true,
      size = 10,
      gap = 2,
      palette = {
        on: "currentColor",
        off: "var(--muted-foreground)",
      },
      brightness = 1,
      ariaLabel,
      onFrame,
      mode = "default",
      levels,
      className,
      ...props
    },
    ref
  ) => {
    const { frameIndex } = useAnimation(frames, {
      fps,
      autoplay: autoplay && !pattern,
      loop,
      onFrame,
    })

    const currentFrame = useMemo(() => {
      if (mode === "vu" && levels && levels.length > 0) {
        return ensureFrameSize(vu(cols, levels), rows, cols)
      }

      if (pattern) {
        return ensureFrameSize(pattern, rows, cols)
      }

      if (frames && frames.length > 0) {
        return ensureFrameSize(frames[frameIndex] || frames[0], rows, cols)
      }

      return ensureFrameSize([], rows, cols)
    }, [pattern, frames, frameIndex, rows, cols, mode, levels])

    const cellPositions = useMemo(() => {
      const positions: CellPosition[][] = []

      for (let row = 0; row < rows; row++) {
        positions[row] = []
        for (let col = 0; col < cols; col++) {
          positions[row][col] = {
            x: col * (size + gap),
            y: row * (size + gap),
          }
        }
      }

      return positions
    }, [rows, cols, size, gap])

    const svgDimensions = useMemo(() => {
      return {
        width: cols * (size + gap) - gap,
        height: rows * (size + gap) - gap,
      }
    }, [rows, cols, size, gap])

    const isAnimating = !pattern && frames && frames.length > 0

    return (
      <div
        ref={ref}
        role="img"
        aria-label={ariaLabel ?? "matrix display"}
        aria-live={isAnimating ? "polite" : undefined}
        className={cn("relative inline-block", className)}
        style={
          {
            "--matrix-on": palette.on,
            "--matrix-off": palette.off,
            "--matrix-gap": `${gap}px`,
            "--matrix-size": `${size}px`,
          } as React.CSSProperties
        }
        {...props}
      >
        <svg
          width={svgDimensions.width}
          height={svgDimensions.height}
          viewBox={`0 0 ${svgDimensions.width} ${svgDimensions.height}`}
          xmlns="http://www.w3.org/2000/svg"
          className="block"
          style={{ overflow: "visible" }}
        >
          <defs>
            <radialGradient id="matrix-pixel-on" cx="50%" cy="50%" r="50%">
              <stop offset="0%" stopColor="var(--matrix-on)" stopOpacity="1" />
              <stop
                offset="70%"
                stopColor="var(--matrix-on)"
                stopOpacity="0.85"
              />
              <stop
                offset="100%"
                stopColor="var(--matrix-on)"
                stopOpacity="0.6"
              />
            </radialGradient>

            <radialGradient id="matrix-pixel-off" cx="50%" cy="50%" r="50%">
              <stop
                offset="0%"
                stopColor="var(--muted-foreground)"
                stopOpacity="1"
              />
              <stop
                offset="100%"
                stopColor="var(--muted-foreground)"
                stopOpacity="0.7"
              />
            </radialGradient>

            <filter
              id="matrix-glow"
              x="-50%"
              y="-50%"
              width="200%"
              height="200%"
            >
              <feGaussianBlur stdDeviation="2" result="blur" />
              <feComposite in="SourceGraphic" in2="blur" operator="over" />
            </filter>
          </defs>

          <style>
            {`
              .matrix-pixel {
                transition: opacity 300ms ease-out, transform 150ms ease-out;
                transform-origin: center;
                transform-box: fill-box;
              }
              .matrix-pixel-active {
                filter: url(#matrix-glow);
              }
            `}
          </style>

          {currentFrame.map((row, rowIndex) =>
            row.map((value, colIndex) => {
              const pos = cellPositions[rowIndex]?.[colIndex]
              if (!pos) return null

              const opacity = clamp(brightness * value)
              const isActive = opacity > 0.5
              const isOn = opacity > 0.05
              const fill = isOn
                ? "url(#matrix-pixel-on)"
                : "url(#matrix-pixel-off)"

              const scale = isActive ? 1.1 : 1
              const radius = (size / 2) * 0.9

              return (
                <circle
                  key={`${rowIndex}-${colIndex}`}
                  className={cn(
                    "matrix-pixel",
                    isActive && "matrix-pixel-active",
                    !isOn && "opacity-20 dark:opacity-[0.1]"
                  )}
                  cx={pos.x + size / 2}
                  cy={pos.y + size / 2}
                  r={radius}
                  fill={fill}
                  opacity={isOn ? opacity : 0.1}
                  style={{
                    transform: `scale(${scale})`,
                  }}
                />
              )
            })
          )}
        </svg>
      </div>
    )
  }
)

Matrix.displayName = "Matrix"
```

---

### Message <a name="message"></a>

Composable message components with avatar, content variants, and automatic styling for user and assistant messages.

- **Registry Key**: `message`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/message.json
```

#### Dependencies

- **NPM Packages**:
  - `class-variance-authority`
- **Local Registry Dependencies**:
  - `avatar`

#### Demo Code

##### File: `examples/message-demo.tsx`

```tsx
"use client"

import { useEffect, useState } from "react"

import { Message, MessageContent } from "@/components/ui/message"
import { Orb } from "@/components/ui/orb"
import { Response } from "@/components/ui/response"

const assistantMessageTokens = [
  "To",
  " create",
  " a",
  " new",
  " agent",
  " with",
  " **",
  "ElevenLabs",
  " Agents",
  "**",
  ",",
  " head",
  " to",
  " this",
  " link",
  ":",
  " ",
  "[",
  "https://elevenlabs.io/app/agents",
  "](",
  "https://elevenlabs.io/app/agents",
  ")",
  ".",
  "\n\n",
  "1.",
  " Sign",
  " in",
  " to",
  " your",
  " ElevenLabs",
  " account",
  ".",
  "\n",
  "2.",
  " Click",
  " **New",
  " Agent**",
  " to",
  " start",
  ".",
  "\n",
  "3.",
  " Give",
  " your",
  " agent",
  " a",
  " name",
  " and",
  " description",
  ".",
  "\n",
  "4.",
  " Configure",
  " its",
  " behavior",
  ",",
  " knowledge",
  " sources",
  ",",
  " and",
  " voice",
  ".",
  "\n",
  "5.",
  " Save",
  " it",
  " —",
  " and",
  " your",
  " agent",
  " is",
  " ready",
  " to",
  " use",
  ".",
]

const Example = () => {
  const [content, setContent] = useState("\u200B")
  const [isStreaming, setIsStreaming] = useState(false)

  useEffect(() => {
    let currentContent = ""
    let index = 0

    const startTimeout = setTimeout(() => {
      setIsStreaming(true)
    }, 500)

    const interval = setInterval(() => {
      if (index < assistantMessageTokens.length) {
        currentContent += assistantMessageTokens[index]
        setContent(currentContent)
        index++
      } else {
        clearInterval(interval)
        setIsStreaming(false)
      }
    }, 100)

    return () => {
      clearInterval(interval)
      clearTimeout(startTimeout)
    }
  }, [])

  return (
    <>
      <style jsx global>{`
        .message-demo-lists ol,
        .message-demo-lists ul {
          padding-left: 1.25rem !important;
        }
        .message-demo-lists li {
          margin-left: 0 !important;
        }
      `}</style>
      <div className="flex h-full max-h-[400px] w-full max-w-2xl flex-col overflow-hidden">
        <div className="flex flex-col gap-4 overflow-y-auto px-4 py-4">
          <div className="flex-shrink-0">
            <Message from="user">
              <MessageContent>
                <Response>How do I create an agent?</Response>
              </MessageContent>
            </Message>
          </div>
          <div className="message-demo-lists flex-shrink-0">
            <Message from="assistant">
              <MessageContent>
                <Response>{content}</Response>
              </MessageContent>
              <div className="ring-border size-8 overflow-hidden rounded-full ring-1">
                <Orb
                  className="h-full w-full"
                  agentState={isStreaming ? "talking" : null}
                />
              </div>
            </Message>
          </div>
        </div>
      </div>
    </>
  )
}

export default Example
```

#### Component Source Code

##### File: `components/ui/message.tsx`

```tsx
import type { ComponentProps, HTMLAttributes } from "react"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/components/ui/avatar"

export type MessageProps = HTMLAttributes<HTMLDivElement> & {
  from: "user" | "assistant"
}

export const Message = ({ className, from, ...props }: MessageProps) => (
  <div
    className={cn(
      "group flex w-full items-end justify-end gap-2 py-4",
      from === "user" ? "is-user" : "is-assistant flex-row-reverse justify-end",
      className
    )}
    {...props}
  />
)

const messageContentVariants = cva(
  "is-user:dark flex flex-col gap-2 overflow-hidden rounded-lg text-sm",
  {
    variants: {
      variant: {
        contained: [
          "max-w-[80%] px-4 py-3",
          "group-[.is-user]:bg-primary group-[.is-user]:text-primary-foreground",
          "group-[.is-assistant]:bg-secondary group-[.is-assistant]:text-foreground",
        ],
        flat: [
          "group-[.is-user]:max-w-[80%] group-[.is-user]:bg-secondary group-[.is-user]:px-4 group-[.is-user]:py-3 group-[.is-user]:text-foreground",
          "group-[.is-assistant]:text-foreground",
        ],
      },
    },
    defaultVariants: {
      variant: "contained",
    },
  }
)

export type MessageContentProps = HTMLAttributes<HTMLDivElement> &
  VariantProps<typeof messageContentVariants>

export const MessageContent = ({
  children,
  className,
  variant,
  ...props
}: MessageContentProps) => (
  <div
    className={cn(messageContentVariants({ variant, className }))}
    {...props}
  >
    {children}
  </div>
)

export type MessageAvatarProps = ComponentProps<typeof Avatar> & {
  src: string
  name?: string
}

export const MessageAvatar = ({
  src,
  name,
  className,
  ...props
}: MessageAvatarProps) => (
  <Avatar className={cn("ring-border size-8 ring-1", className)} {...props}>
    <AvatarImage alt="" className="mt-0 mb-0" src={src} />
    <AvatarFallback>{name?.slice(0, 2) || "ME"}</AvatarFallback>
  </Avatar>
)
```

---

### Mic Selector <a name="mic-selector"></a>

Microphone input selector with device management.

- **Registry Key**: `mic-selector`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/mic-selector.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `button`
  - `card`
  - `dropdown-menu`
  - `https://ui.elevenlabs.io/r/live-waveform.json`

#### How to Use

```tsx
interface AudioDevice {
  deviceId: string
  label: string
  groupId: string
}
```

#### Demo Code

##### File: `examples/mic-selector-demo.tsx`

```tsx
"use client"

import { useCallback, useEffect, useRef, useState } from "react"
import { Disc, Pause, Play, Trash2 } from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Card } from "@/components/ui/card"
import { LiveWaveform } from "@/components/ui/live-waveform"
import { MicSelector } from "@/components/ui/mic-selector"
import { Separator } from "@/components/ui/separator"

type RecordingState = "idle" | "loading" | "recording" | "recorded" | "playing"

export default function MicSelectorDemo() {
  const [selectedDevice, setSelectedDevice] = useState<string>("")
  const [isMuted, setIsMuted] = useState(false)
  const [state, setState] = useState<RecordingState>("idle")
  const [audioBlob, setAudioBlob] = useState<Blob | null>(null)

  const mediaRecorderRef = useRef<MediaRecorder | null>(null)
  const audioChunksRef = useRef<Blob[]>([])
  const audioElementRef = useRef<HTMLAudioElement | null>(null)

  const startRecording = useCallback(async () => {
    try {
      setState("loading")

      const stream = await navigator.mediaDevices.getUserMedia({
        audio: selectedDevice ? { deviceId: { exact: selectedDevice } } : true,
      })

      const mediaRecorder = new MediaRecorder(stream)
      mediaRecorderRef.current = mediaRecorder
      audioChunksRef.current = []

      mediaRecorder.ondataavailable = (event) => {
        if (event.data.size > 0) {
          audioChunksRef.current.push(event.data)
        }
      }

      mediaRecorder.onstop = () => {
        const blob = new Blob(audioChunksRef.current, { type: "audio/webm" })
        setAudioBlob(blob)
        stream.getTracks().forEach((track) => track.stop())
        setState("recorded")
      }

      mediaRecorder.start()
      setState("recording")
    } catch (error) {
      console.error("Error starting recording:", error)
      setState("idle")
    }
  }, [selectedDevice])

  const stopRecording = useCallback(() => {
    if (mediaRecorderRef.current && state === "recording") {
      mediaRecorderRef.current.stop()
    }
  }, [state])

  const playRecording = useCallback(() => {
    if (!audioBlob) return

    const audio = new Audio(URL.createObjectURL(audioBlob))
    audioElementRef.current = audio

    audio.onended = () => {
      setState("recorded")
    }

    audio.play()
    setState("playing")
  }, [audioBlob])

  const pausePlayback = useCallback(() => {
    if (audioElementRef.current) {
      audioElementRef.current.pause()
      setState("recorded")
    }
  }, [])

  const restart = useCallback(() => {
    if (audioElementRef.current) {
      audioElementRef.current.pause()
      audioElementRef.current = null
    }
    setAudioBlob(null)
    audioChunksRef.current = []
    setState("idle")
  }, [])

  // Stop recording when muted
  useEffect(() => {
    if (isMuted && state === "recording") {
      stopRecording()
    }
  }, [isMuted, state, stopRecording])

  // Cleanup on unmount
  useEffect(() => {
    return () => {
      if (mediaRecorderRef.current) {
        mediaRecorderRef.current.stop()
      }
      if (audioElementRef.current) {
        audioElementRef.current.pause()
      }
    }
  }, [])

  const showWaveform = state === "recording" && !isMuted
  const showProcessing = state === "loading" || state === "playing"
  const showRecorded = state === "recorded"

  return (
    <div className="flex min-h-[200px] w-full items-center justify-center p-4">
      <Card className="m-0 w-full max-w-2xl border p-0 shadow-lg">
        <div className="flex w-full flex-wrap items-center justify-between gap-2 p-2">
          <div className="h-8 w-full min-w-0 flex-1 md:w-[200px] md:flex-none">
            <div
              className={cn(
                "flex h-full items-center gap-2 rounded-md py-1",
                "bg-foreground/5 text-foreground/70"
              )}
            >
              <div className="h-full min-w-0 flex-1">
                <div className="relative flex h-full w-full shrink-0 items-center justify-center overflow-hidden rounded-sm">
                  <LiveWaveform
                    key={state}
                    active={showWaveform}
                    processing={showProcessing}
                    deviceId={selectedDevice}
                    barWidth={3}
                    barGap={1}
                    barRadius={4}
                    fadeEdges={true}
                    fadeWidth={24}
                    sensitivity={1.8}
                    smoothingTimeConstant={0.85}
                    height={20}
                    mode="scrolling"
                    className={cn(
                      "h-full w-full transition-opacity duration-300",
                      state === "idle" && "opacity-0"
                    )}
                  />
                  {state === "idle" && (
                    <div className="absolute inset-0 flex items-center justify-center">
                      <span className="text-foreground/50 text-xs font-medium">
                        Start Recording
                      </span>
                    </div>
                  )}
                  {showRecorded && (
                    <div className="absolute inset-0 flex items-center justify-center">
                      <span className="text-foreground/50 text-xs font-medium">
                        Ready to Play
                      </span>
                    </div>
                  )}
                </div>
              </div>
            </div>
          </div>
          <div className="flex w-full flex-wrap items-center justify-center gap-1 md:w-auto">
            <MicSelector
              value={selectedDevice}
              onValueChange={setSelectedDevice}
              muted={isMuted}
              onMutedChange={setIsMuted}
              disabled={state === "recording" || state === "loading"}
            />
            <Separator orientation="vertical" className="mx-1 -my-2.5" />
            <div className="flex">
              {state === "idle" && (
                <Button
                  variant="ghost"
                  size="icon"
                  onClick={startRecording}
                  disabled={isMuted}
                  aria-label="Start recording"
                >
                  <Disc className="size-5" />
                </Button>
              )}
              {(state === "loading" || state === "recording") && (
                <Button
                  variant="ghost"
                  size="icon"
                  onClick={stopRecording}
                  disabled={state === "loading"}
                  aria-label="Stop recording"
                >
                  <Pause className="size-5" />
                </Button>
              )}
              {showRecorded && (
                <Button
                  variant="ghost"
                  size="icon"
                  onClick={playRecording}
                  aria-label="Play recording"
                >
                  <Play className="size-5" />
                </Button>
              )}
              {state === "playing" && (
                <Button
                  variant="ghost"
                  size="icon"
                  onClick={pausePlayback}
                  aria-label="Pause playback"
                >
                  <Pause className="size-5" />
                </Button>
              )}
              <Separator orientation="vertical" className="mx-1 -my-2.5" />
              <Button
                variant="ghost"
                size="icon"
                onClick={restart}
                disabled={
                  state === "idle" ||
                  state === "loading" ||
                  state === "recording"
                }
                aria-label="Delete recording"
              >
                <Trash2 className="size-5" />
              </Button>
            </div>
          </div>
        </div>
      </Card>
    </div>
  )
}
```

#### Component Source Code

##### File: `components/ui/mic-selector.tsx`

```tsx
"use client"

import { useCallback, useEffect, useState } from "react"
import { Check, ChevronsUpDown, Mic, MicOff } from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"
import { LiveWaveform } from "@/components/ui/live-waveform"

export interface AudioDevice {
  deviceId: string
  label: string
  groupId: string
}

export interface MicSelectorProps {
  value?: string
  onValueChange?: (deviceId: string) => void
  muted?: boolean
  onMutedChange?: (muted: boolean) => void
  disabled?: boolean
  className?: string
}

export function MicSelector({
  value,
  onValueChange,
  muted,
  onMutedChange,
  disabled,
  className,
}: MicSelectorProps) {
  const { devices, loading, error, hasPermission, loadDevices } =
    useAudioDevices()
  const [selectedDevice, setSelectedDevice] = useState<string>(value || "")
  const [internalMuted, setInternalMuted] = useState(false)
  const [isDropdownOpen, setIsDropdownOpen] = useState(false)

  // Use controlled muted if provided, otherwise use internal state
  const isMuted = muted !== undefined ? muted : internalMuted

  // Update internal state when controlled value changes
  useEffect(() => {
    if (value !== undefined) {
      setSelectedDevice(value)
    }
  }, [value])

  // Select first device by default
  const defaultDeviceId = devices[0]?.deviceId || ""
  useEffect(() => {
    if (!selectedDevice && defaultDeviceId) {
      const newDevice = defaultDeviceId
      setSelectedDevice(newDevice)
      onValueChange?.(newDevice)
    }
  }, [defaultDeviceId, selectedDevice, onValueChange])

  const currentDevice = devices.find((d) => d.deviceId === selectedDevice) ||
    devices[0] || {
      label: loading ? "Loading..." : "No microphone",
      deviceId: "",
    }

  const handleDeviceSelect = (deviceId: string, e?: React.MouseEvent) => {
    e?.preventDefault()
    setSelectedDevice(deviceId)
    onValueChange?.(deviceId)
  }

  const handleDropdownOpenChange = async (open: boolean) => {
    setIsDropdownOpen(open)
    if (open && !hasPermission && !loading) {
      await loadDevices()
    }
  }

  const toggleMute = () => {
    const newMuted = !isMuted
    if (muted === undefined) {
      setInternalMuted(newMuted)
    }
    onMutedChange?.(newMuted)
  }

  const isPreviewActive = isDropdownOpen && !isMuted

  return (
    <DropdownMenu onOpenChange={handleDropdownOpenChange}>
      <DropdownMenuTrigger asChild>
        <Button
          variant="ghost"
          size="sm"
          className={cn(
            "hover:bg-accent flex w-40 min-w-0 shrink cursor-pointer items-center gap-1.5 sm:w-48",
            className
          )}
          disabled={loading || disabled}
        >
          {isMuted ? (
            <MicOff className="h-4 w-4 flex-shrink-0" />
          ) : (
            <Mic className="h-4 w-4 flex-shrink-0" />
          )}
          <span className="min-w-0 flex-1 truncate text-left text-xs sm:text-sm">
            {currentDevice.label}
          </span>
          <ChevronsUpDown className="h-3 w-3 flex-shrink-0" />
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent align="center" side="top" className="w-72">
        {loading ? (
          <DropdownMenuItem disabled>Loading devices...</DropdownMenuItem>
        ) : error ? (
          <DropdownMenuItem disabled>Error: {error}</DropdownMenuItem>
        ) : (
          devices.map((device) => (
            <DropdownMenuItem
              key={device.deviceId}
              onClick={(e) => handleDeviceSelect(device.deviceId, e)}
              onSelect={(e) => e.preventDefault()}
              className="flex items-center justify-between"
            >
              <span className="truncate">{device.label}</span>
              {selectedDevice === device.deviceId && (
                <Check className="h-4 w-4 flex-shrink-0" />
              )}
            </DropdownMenuItem>
          ))
        )}
        {devices.length > 0 && (
          <>
            <DropdownMenuSeparator />
            <div className="flex items-center gap-2 p-2">
              <Button
                variant="ghost"
                size="sm"
                onClick={(e) => {
                  e.preventDefault()
                  toggleMute()
                }}
                className="h-8 gap-2"
              >
                {isMuted ? (
                  <MicOff className="h-4 w-4" />
                ) : (
                  <Mic className="h-4 w-4" />
                )}
                <span className="text-sm">{isMuted ? "Unmute" : "Mute"}</span>
              </Button>
              <div className="bg-accent ml-auto w-16 overflow-hidden rounded-md p-1.5">
                <LiveWaveform
                  active={isPreviewActive}
                  deviceId={selectedDevice || defaultDeviceId}
                  mode="static"
                  height={15}
                  barWidth={3}
                  barGap={1}
                />
              </div>
            </div>
          </>
        )}
      </DropdownMenuContent>
    </DropdownMenu>
  )
}

export function useAudioDevices() {
  const [devices, setDevices] = useState<AudioDevice[]>([])
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState<string | null>(null)
  const [hasPermission, setHasPermission] = useState(false)

  const loadDevicesWithoutPermission = useCallback(async () => {
    try {
      setLoading(true)
      setError(null)

      const deviceList = await navigator.mediaDevices.enumerateDevices()

      const audioInputs = deviceList
        .filter((device) => device.kind === "audioinput")
        .map((device) => {
          let cleanLabel =
            device.label || `Microphone ${device.deviceId.slice(0, 8)}`
          cleanLabel = cleanLabel.replace(/\s*\([^)]*\)/g, "").trim()

          return {
            deviceId: device.deviceId,
            label: cleanLabel,
            groupId: device.groupId,
          }
        })

      setDevices(audioInputs)
    } catch (err) {
      setError(
        err instanceof Error ? err.message : "Failed to get audio devices"
      )
      console.error("Error getting audio devices:", err)
    } finally {
      setLoading(false)
    }
  }, [])

  const loadDevicesWithPermission = useCallback(async () => {
    if (loading) return

    try {
      setLoading(true)
      setError(null)

      const tempStream = await navigator.mediaDevices.getUserMedia({
        audio: true,
      })
      tempStream.getTracks().forEach((track) => track.stop())

      const deviceList = await navigator.mediaDevices.enumerateDevices()

      const audioInputs = deviceList
        .filter((device) => device.kind === "audioinput")
        .map((device) => {
          let cleanLabel =
            device.label || `Microphone ${device.deviceId.slice(0, 8)}`
          cleanLabel = cleanLabel.replace(/\s*\([^)]*\)/g, "").trim()

          return {
            deviceId: device.deviceId,
            label: cleanLabel,
            groupId: device.groupId,
          }
        })

      setDevices(audioInputs)
      setHasPermission(true)
    } catch (err) {
      setError(
        err instanceof Error ? err.message : "Failed to get audio devices"
      )
      console.error("Error getting audio devices:", err)
    } finally {
      setLoading(false)
    }
  }, [loading])

  useEffect(() => {
    loadDevicesWithoutPermission()
  }, [loadDevicesWithoutPermission])

  useEffect(() => {
    const handleDeviceChange = () => {
      if (hasPermission) {
        loadDevicesWithPermission()
      } else {
        loadDevicesWithoutPermission()
      }
    }

    navigator.mediaDevices.addEventListener("devicechange", handleDeviceChange)

    return () => {
      navigator.mediaDevices.removeEventListener(
        "devicechange",
        handleDeviceChange
      )
    }
  }, [hasPermission, loadDevicesWithPermission, loadDevicesWithoutPermission])

  return {
    devices,
    loading,
    error,
    hasPermission,
    loadDevices: loadDevicesWithPermission,
  }
}
```

---

### Orb <a name="orb"></a>

A 3D animated orb with audio reactivity, custom colors, and agent state visualization built with Three.js.

- **Registry Key**: `orb`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/orb.json
```

#### Dependencies

- **NPM Packages**:
  - `@react-three/drei`
  - `@react-three/fiber`
  - `three`
  - `@types/three`

#### How to Use

```tsx
type AgentState = null | "thinking" | "listening" | "talking"
```

#### Demo Code

##### File: `examples/orb-demo.tsx`

```tsx
"use client"

import { useState } from "react"

import { Button } from "@/components/ui/button"
import { AgentState, Orb } from "@/components/ui/orb"

let ORBS: [string, string][] = [
  ["#CADCFC", "#A0B9D1"],
  ["#F6E7D8", "#E0CFC2"],
  ["#E5E7EB", "#9CA3AF"],
]

export default function OrbDemo({ small = false }: { small?: boolean }) {
  const [agent, setAgent] = useState<AgentState>(null)

  ORBS = small ? [ORBS[0]] : ORBS

  return (
    <div className="bg-card w-full rounded-lg border p-6">
      <div className="mb-4">
        <h3 className="text-lg font-semibold">Agent Orbs</h3>
        <p className="text-muted-foreground text-sm">
          Interactive orb visualization with agent states
        </p>
      </div>

      <div className="space-y-4">
        <div className="flex justify-center gap-8">
          {ORBS.map((colors, index) => (
            <div
              key={index}
              className={`relative ${index === 1 ? "block md:block" : "hidden md:block"}`}
            >
              <div className="bg-muted relative h-32 w-32 rounded-full p-1 shadow-[inset_0_2px_8px_rgba(0,0,0,0.1)] dark:shadow-[inset_0_2px_8px_rgba(0,0,0,0.5)]">
                <div className="bg-background h-full w-full overflow-hidden rounded-full shadow-[inset_0_0_12px_rgba(0,0,0,0.05)] dark:shadow-[inset_0_0_12px_rgba(0,0,0,0.3)]">
                  <Orb
                    colors={colors}
                    seed={(index + 1) * 1000}
                    agentState={agent}
                  />
                </div>
              </div>
            </div>
          ))}
        </div>

        <div className="flex flex-wrap justify-center gap-2">
          <Button
            size="sm"
            variant="outline"
            onClick={() => setAgent(null)}
            disabled={agent === null}
          >
            Idle
          </Button>
          <Button
            size="sm"
            variant="outline"
            onClick={() => setAgent("listening")}
            disabled={agent === "listening"}
          >
            Listening
          </Button>
          <Button
            size="sm"
            variant="outline"
            disabled={agent === "talking"}
            onClick={() => setAgent("talking")}
          >
            Talking
          </Button>
        </div>
      </div>
    </div>
  )
}
```

#### Props & API Reference

##### `Props` Props

| Prop            | Type                          | Default                  | Description                                           |
| --------------- | ----------------------------- | ------------------------ | ----------------------------------------------------- |
| colors          | `[string, string]`            | `["#CADCFC", "#A0B9D1"]` | Two color values for the gradient                     |
| colorsRef       | `RefObject<[string, string]>` | -                        | Ref for dynamic color updates                         |
| resizeDebounce  | `number`                      | `100`                    | Canvas resize debounce in ms                          |
| seed            | `number`                      | Random                   | Seed for consistent animations                        |
| agentState      | `AgentState`                  | `null`                   | Agent state: null, "thinking", "listening", "talking" |
| volumeMode      | `"auto" \| "manual"`          | `"auto"`                 | Volume control mode                                   |
| manualInput     | `number`                      | -                        | Manual input volume (0-1)                             |
| manualOutput    | `number`                      | -                        | Manual output volume (0-1)                            |
| inputVolumeRef  | `RefObject<number>`           | -                        | Ref for input volume                                  |
| outputVolumeRef | `RefObject<number>`           | -                        | Ref for output volume                                 |
| getInputVolume  | `() => number`                | -                        | Function returning input volume (0-1)                 |
| getOutputVolume | `() => number`                | -                        | Function returning output volume (0-1)                |
| className       | `string`                      | -                        | Custom CSS class                                      |

#### Component Source Code

##### File: `components/ui/orb.tsx`

```tsx
"use client"

import { useEffect, useMemo, useRef } from "react"
import { useTexture } from "@react-three/drei"
import { Canvas, useFrame, useThree } from "@react-three/fiber"
import * as THREE from "three"

export type AgentState = null | "thinking" | "listening" | "talking"

type OrbProps = {
  colors?: [string, string]
  colorsRef?: React.RefObject<[string, string]>
  resizeDebounce?: number
  seed?: number
  agentState?: AgentState
  volumeMode?: "auto" | "manual"
  manualInput?: number
  manualOutput?: number
  inputVolumeRef?: React.RefObject<number>
  outputVolumeRef?: React.RefObject<number>
  getInputVolume?: () => number
  getOutputVolume?: () => number
  className?: string
}

export function Orb({
  colors = ["#CADCFC", "#A0B9D1"],
  colorsRef,
  resizeDebounce = 100,
  seed,
  agentState = null,
  volumeMode = "auto",
  manualInput,
  manualOutput,
  inputVolumeRef,
  outputVolumeRef,
  getInputVolume,
  getOutputVolume,
  className,
}: OrbProps) {
  return (
    <div className={className ?? "relative h-full w-full"}>
      <Canvas
        resize={{ debounce: resizeDebounce }}
        gl={{
          alpha: true,
          antialias: true,
          premultipliedAlpha: true,
        }}
      >
        <Scene
          colors={colors}
          colorsRef={colorsRef}
          seed={seed}
          agentState={agentState}
          volumeMode={volumeMode}
          manualInput={manualInput}
          manualOutput={manualOutput}
          inputVolumeRef={inputVolumeRef}
          outputVolumeRef={outputVolumeRef}
          getInputVolume={getInputVolume}
          getOutputVolume={getOutputVolume}
        />
      </Canvas>
    </div>
  )
}

function Scene({
  colors,
  colorsRef,
  seed,
  agentState,
  volumeMode,
  manualInput,
  manualOutput,
  inputVolumeRef,
  outputVolumeRef,
  getInputVolume,
  getOutputVolume,
}: {
  colors: [string, string]
  colorsRef?: React.RefObject<[string, string]>
  seed?: number
  agentState: AgentState
  volumeMode: "auto" | "manual"
  manualInput?: number
  manualOutput?: number
  inputVolumeRef?: React.RefObject<number>
  outputVolumeRef?: React.RefObject<number>
  getInputVolume?: () => number
  getOutputVolume?: () => number
}) {
  const { gl } = useThree()
  const circleRef =
    useRef<THREE.Mesh<THREE.CircleGeometry, THREE.ShaderMaterial>>(null)
  const initialColorsRef = useRef<[string, string]>(colors)
  const targetColor1Ref = useRef(new THREE.Color(colors[0]))
  const targetColor2Ref = useRef(new THREE.Color(colors[1]))
  const animSpeedRef = useRef(0.1)
  const perlinNoiseTexture = useTexture(
    "https://storage.googleapis.com/eleven-public-cdn/images/perlin-noise.png"
  )

  const agentRef = useRef<AgentState>(agentState)
  const modeRef = useRef<"auto" | "manual">(volumeMode)
  const manualInRef = useRef<number>(manualInput ?? 0)
  const manualOutRef = useRef<number>(manualOutput ?? 0)
  const curInRef = useRef(0)
  const curOutRef = useRef(0)

  useEffect(() => {
    agentRef.current = agentState
  }, [agentState])

  useEffect(() => {
    modeRef.current = volumeMode
  }, [volumeMode])

  useEffect(() => {
    manualInRef.current = clamp01(
      manualInput ?? inputVolumeRef?.current ?? getInputVolume?.() ?? 0
    )
  }, [manualInput, inputVolumeRef, getInputVolume])

  useEffect(() => {
    manualOutRef.current = clamp01(
      manualOutput ?? outputVolumeRef?.current ?? getOutputVolume?.() ?? 0
    )
  }, [manualOutput, outputVolumeRef, getOutputVolume])

  const random = useMemo(
    () => splitmix32(seed ?? Math.floor(Math.random() * 2 ** 32)),
    [seed]
  )
  const offsets = useMemo(
    () =>
      new Float32Array(Array.from({ length: 7 }, () => random() * Math.PI * 2)),
    [random]
  )

  useEffect(() => {
    targetColor1Ref.current = new THREE.Color(colors[0])
    targetColor2Ref.current = new THREE.Color(colors[1])
  }, [colors])

  useEffect(() => {
    const apply = () => {
      if (!circleRef.current) return
      const isDark = document.documentElement.classList.contains("dark")
      circleRef.current.material.uniforms.uInverted.value = isDark ? 1 : 0
    }

    apply()

    const observer = new MutationObserver(apply)
    observer.observe(document.documentElement, {
      attributes: true,
      attributeFilter: ["class"],
    })
    return () => observer.disconnect()
  }, [])

  useFrame((_, delta: number) => {
    const mat = circleRef.current?.material
    if (!mat) return
    const live = colorsRef?.current
    if (live) {
      if (live[0]) targetColor1Ref.current.set(live[0])
      if (live[1]) targetColor2Ref.current.set(live[1])
    }
    const u = mat.uniforms
    u.uTime.value += delta * 0.5

    if (u.uOpacity.value < 1) {
      u.uOpacity.value = Math.min(1, u.uOpacity.value + delta * 2)
    }

    let targetIn = 0
    let targetOut = 0.3
    if (modeRef.current === "manual") {
      targetIn = clamp01(
        manualInput ?? inputVolumeRef?.current ?? getInputVolume?.() ?? 0
      )
      targetOut = clamp01(
        manualOutput ?? outputVolumeRef?.current ?? getOutputVolume?.() ?? 0
      )
    } else {
      const t = u.uTime.value * 2
      if (agentRef.current === null) {
        targetIn = 0
        targetOut = 0.3
      } else if (agentRef.current === "listening") {
        targetIn = clamp01(0.55 + Math.sin(t * 3.2) * 0.35)
        targetOut = 0.45
      } else if (agentRef.current === "talking") {
        targetIn = clamp01(0.65 + Math.sin(t * 4.8) * 0.22)
        targetOut = clamp01(0.75 + Math.sin(t * 3.6) * 0.22)
      } else {
        const base = 0.38 + 0.07 * Math.sin(t * 0.7)
        const wander = 0.05 * Math.sin(t * 2.1) * Math.sin(t * 0.37 + 1.2)
        targetIn = clamp01(base + wander)
        targetOut = clamp01(0.48 + 0.12 * Math.sin(t * 1.05 + 0.6))
      }
    }

    curInRef.current += (targetIn - curInRef.current) * 0.2
    curOutRef.current += (targetOut - curOutRef.current) * 0.2

    const targetSpeed = 0.1 + (1 - Math.pow(curOutRef.current - 1, 2)) * 0.9
    animSpeedRef.current += (targetSpeed - animSpeedRef.current) * 0.12

    u.uAnimation.value += delta * animSpeedRef.current
    u.uInputVolume.value = curInRef.current
    u.uOutputVolume.value = curOutRef.current
    u.uColor1.value.lerp(targetColor1Ref.current, 0.08)
    u.uColor2.value.lerp(targetColor2Ref.current, 0.08)
  })

  useEffect(() => {
    const canvas = gl.domElement
    const onContextLost = (event: Event) => {
      event.preventDefault()
      setTimeout(() => {
        gl.forceContextRestore()
      }, 1)
    }
    canvas.addEventListener("webglcontextlost", onContextLost, false)
    return () =>
      canvas.removeEventListener("webglcontextlost", onContextLost, false)
  }, [gl])

  const uniforms = useMemo(() => {
    perlinNoiseTexture.wrapS = THREE.RepeatWrapping
    perlinNoiseTexture.wrapT = THREE.RepeatWrapping
    const isDark =
      typeof document !== "undefined" &&
      document.documentElement.classList.contains("dark")
    return {
      uColor1: new THREE.Uniform(new THREE.Color(initialColorsRef.current[0])),
      uColor2: new THREE.Uniform(new THREE.Color(initialColorsRef.current[1])),
      uOffsets: { value: offsets },
      uPerlinTexture: new THREE.Uniform(perlinNoiseTexture),
      uTime: new THREE.Uniform(0),
      uAnimation: new THREE.Uniform(0.1),
      uInverted: new THREE.Uniform(isDark ? 1 : 0),
      uInputVolume: new THREE.Uniform(0),
      uOutputVolume: new THREE.Uniform(0),
      uOpacity: new THREE.Uniform(0),
    }
  }, [perlinNoiseTexture, offsets])

  return (
    <mesh ref={circleRef}>
      <circleGeometry args={[3.5, 64]} />
      <shaderMaterial
        uniforms={uniforms}
        fragmentShader={fragmentShader}
        vertexShader={vertexShader}
        transparent={true}
      />
    </mesh>
  )
}

function splitmix32(a: number) {
  return function () {
    a |= 0
    a = (a + 0x9e3779b9) | 0
    let t = a ^ (a >>> 16)
    t = Math.imul(t, 0x21f0aaad)
    t = t ^ (t >>> 15)
    t = Math.imul(t, 0x735a2d97)
    return ((t = t ^ (t >>> 15)) >>> 0) / 4294967296
  }
}

function clamp01(n: number) {
  if (!Number.isFinite(n)) return 0
  return Math.min(1, Math.max(0, n))
}
const vertexShader = /* glsl */ `
uniform float uTime;
uniform sampler2D uPerlinTexture;
varying vec2 vUv;

void main() {
  vUv = uv;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}
`

const fragmentShader = /* glsl */ `
uniform float uTime;
uniform float uAnimation;
uniform float uInverted;
uniform float uOffsets[7];
uniform vec3 uColor1;
uniform vec3 uColor2;
uniform float uInputVolume;
uniform float uOutputVolume;
uniform float uOpacity;
uniform sampler2D uPerlinTexture;
varying vec2 vUv;

const float PI = 3.14159265358979323846;

// Draw a single oval with soft edges and calculate its gradient color
bool drawOval(vec2 polarUv, vec2 polarCenter, float a, float b, bool reverseGradient, float softness, out vec4 color) {
    vec2 p = polarUv - polarCenter;
    float oval = (p.x * p.x) / (a * a) + (p.y * p.y) / (b * b);

    float edge = smoothstep(1.0, 1.0 - softness, oval);

    if (edge > 0.0) {
        float gradient = reverseGradient ? (1.0 - (p.x / a + 1.0) / 2.0) : ((p.x / a + 1.0) / 2.0);
        // Flatten gradient toward middle value for more uniform appearance
        gradient = mix(0.5, gradient, 0.1);
        color = vec4(vec3(gradient), 0.85 * edge);
        return true;
    }
    return false;
}

// Map grayscale value to a 4-color ramp (color1, color2, color3, color4)
vec3 colorRamp(float grayscale, vec3 color1, vec3 color2, vec3 color3, vec3 color4) {
    if (grayscale < 0.33) {
        return mix(color1, color2, grayscale * 3.0);
    } else if (grayscale < 0.66) {
        return mix(color2, color3, (grayscale - 0.33) * 3.0);
    } else {
        return mix(color3, color4, (grayscale - 0.66) * 3.0);
    }
}

vec2 hash2(vec2 p) {
    return fract(sin(vec2(dot(p, vec2(127.1, 311.7)), dot(p, vec2(269.5, 183.3)))) * 43758.5453);
}

// 2D noise for the ring
float noise2D(vec2 p) {
    vec2 i = floor(p);
    vec2 f = fract(p);
    
    vec2 u = f * f * (3.0 - 2.0 * f);
    float n = mix(
        mix(dot(hash2(i + vec2(0.0, 0.0)), f - vec2(0.0, 0.0)),
            dot(hash2(i + vec2(1.0, 0.0)), f - vec2(1.0, 0.0)), u.x),
        mix(dot(hash2(i + vec2(0.0, 1.0)), f - vec2(0.0, 1.0)),
            dot(hash2(i + vec2(1.0, 1.0)), f - vec2(1.0, 1.0)), u.x),
        u.y
    );

    return 0.5 + 0.5 * n;
}

float sharpRing(vec3 decomposed, float time) {
    float ringStart = 1.0;
    float ringWidth = 0.3;
    float noiseScale = 5.0;

    float noise = mix(
        noise2D(vec2(decomposed.x, time) * noiseScale),
        noise2D(vec2(decomposed.y, time) * noiseScale),
        decomposed.z
    );

    noise = (noise - 0.5) * 2.5;

    return ringStart + noise * ringWidth * 1.5;
}

float smoothRing(vec3 decomposed, float time) {
    float ringStart = 0.9;
    float ringWidth = 0.2;
    float noiseScale = 6.0;

    float noise = mix(
        noise2D(vec2(decomposed.x, time) * noiseScale),
        noise2D(vec2(decomposed.y, time) * noiseScale),
        decomposed.z
    );

    noise = (noise - 0.5) * 5.0;

    return ringStart + noise * ringWidth;
}

float flow(vec3 decomposed, float time) {
    return mix(
        texture(uPerlinTexture, vec2(time, decomposed.x / 2.0)).r,
        texture(uPerlinTexture, vec2(time, decomposed.y / 2.0)).r,
        decomposed.z
    );
}

void main() {
    // Normalize vUv to be centered around (0.0, 0.0)
    vec2 uv = vUv * 2.0 - 1.0;

    // Convert uv to polar coordinates
    float radius = length(uv);
    float theta = atan(uv.y, uv.x);
    if (theta < 0.0) theta += 2.0 * PI; // Normalize theta to [0, 2*PI]

    // Decomposed angle is used for sampling noise textures without seams:
    // float noise = mix(sample(decomposed.x), sample(decomposed.y), decomposed.z);
    vec3 decomposed = vec3(
        // angle in the range [0, 1]
        theta / (2.0 * PI),
        // angle offset by 180 degrees in the range [1, 2]
        mod(theta / (2.0 * PI) + 0.5, 1.0) + 1.0,
        // mixing factor between two noises
        abs(theta / PI - 1.0)
    );

    // Add noise to the angle for a flow-like distortion (reduced for flatter look)
    float noise = flow(decomposed, radius * 0.03 - uAnimation * 0.2) - 0.5;
    theta += noise * mix(0.08, 0.25, uOutputVolume);

    // Initialize the base color to white
    vec4 color = vec4(1.0, 1.0, 1.0, 1.0);

    // Original parameters for the ovals in polar coordinates
    float originalCenters[7] = float[7](0.0, 0.5 * PI, 1.0 * PI, 1.5 * PI, 2.0 * PI, 2.5 * PI, 3.0 * PI);

    // Parameters for the animated centers in polar coordinates
    float centers[7];
    for (int i = 0; i < 7; i++) {
        centers[i] = originalCenters[i] + 0.5 * sin(uTime / 20.0 + uOffsets[i]);
    }

    float a, b;
    vec4 ovalColor;

    // Check if the pixel is inside any of the ovals
    for (int i = 0; i < 7; i++) {
        float noise = texture(uPerlinTexture, vec2(mod(centers[i] + uTime * 0.05, 1.0), 0.5)).r;
        a = 0.5 + noise * 0.3; // Increased for more coverage
        b = noise * mix(3.5, 2.5, uInputVolume); // Increased height for fuller appearance
        bool reverseGradient = (i % 2 == 1); // Reverse gradient for every second oval

        // Calculate the distance in polar coordinates
        float distTheta = min(
            abs(theta - centers[i]),
            min(
                abs(theta + 2.0 * PI - centers[i]),
                abs(theta - 2.0 * PI - centers[i])
            )
        );
        float distRadius = radius;

        float softness = 0.6; // Increased softness for flatter, less pronounced edges

        // Check if the pixel is inside the oval in polar coordinates
        if (drawOval(vec2(distTheta, distRadius), vec2(0.0, 0.0), a, b, reverseGradient, softness, ovalColor)) {
            // Blend the oval color with the existing color
            color.rgb = mix(color.rgb, ovalColor.rgb, ovalColor.a);
            color.a = max(color.a, ovalColor.a); // Max alpha
        }
    }
    
    // Calculate both noisy rings
    float ringRadius1 = sharpRing(decomposed, uTime * 0.1);
    float ringRadius2 = smoothRing(decomposed, uTime * 0.1);
    
    // Adjust rings based on input volume (reduced for flatter appearance)
    float inputRadius1 = radius + uInputVolume * 0.2;
    float inputRadius2 = radius + uInputVolume * 0.15;
    float opacity1 = mix(0.2, 0.6, uInputVolume);
    float opacity2 = mix(0.15, 0.45, uInputVolume);

    // Blend both rings
    float ringAlpha1 = (inputRadius2 >= ringRadius1) ? opacity1 : 0.0;
    float ringAlpha2 = smoothstep(ringRadius2 - 0.05, ringRadius2 + 0.05, inputRadius1) * opacity2;
    
    float totalRingAlpha = max(ringAlpha1, ringAlpha2);
    
    // Apply screen blend mode for combined rings
    vec3 ringColor = vec3(1.0); // White ring color
    color.rgb = 1.0 - (1.0 - color.rgb) * (1.0 - ringColor * totalRingAlpha);

    // Define colours to ramp against greyscale (could increase the amount of colours in the ramp)
    vec3 color1 = vec3(0.0, 0.0, 0.0); // Black
    vec3 color2 = uColor1; // Darker Color
    vec3 color3 = uColor2; // Lighter Color
    vec3 color4 = vec3(1.0, 1.0, 1.0); // White

    // Convert grayscale color to the color ramp
    float luminance = mix(color.r, 1.0 - color.r, uInverted);
    color.rgb = colorRamp(luminance, color1, color2, color3, color4); // Apply the color ramp

    // Apply fade-in opacity
    color.a *= uOpacity;

    gl_FragColor = color;
}
`
```

---

### Response <a name="response"></a>

Streaming markdown renderer with smooth character-by-character animations for AI responses using Streamdown.

- **Registry Key**: `response`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/response.json
```

#### Dependencies

- **NPM Packages**:
  - `streamdown`

#### Demo Code

##### File: `examples/response-demo.tsx`

```tsx
"use client"

import { useEffect, useState } from "react"

import { Response } from "@/components/ui/response"

const tokens = [
  "### Welcome",
  "\n\n",
  "This",
  " is",
  " a",
  " **rich",
  " markdown",
  "**",
  " showcase",
  " with",
  " multiple",
  " features.",
  "\n\n",
  "---",
  "\n\n",
  "## Data Table",
  "\n\n",
  "| Name",
  " | Role",
  " | Status",
  " |",
  "\n",
  "|------|------|--------|",
  "\n",
  "| Alice",
  " | Engineer",
  " | Active",
  " |",
  "\n",
  "| Bob",
  " | Designer",
  " | Active",
  " |",
  "\n",
  "| Carol",
  " | PM",
  " | Active",
  " |",
  "\n\n",
  "## Inspiration",
  "\n\n",
  "> *Simplicity",
  " is",
  " the",
  " ultimate",
  " sophistication.*",
  "\n",
  "> —",
  " Leonardo",
  " da",
  " Vinci",
  "\n\n",
  "## Inline",
  " and",
  " Block",
  " Code",
  "\n\n",
  "Use",
  " `let",
  " total",
  " =",
  " items.length`",
  " to",
  " count",
  " elements.",
  "\n\n",
  "```",
  "python",
  "\n",
  "def",
  " greet(name):",
  "\n",
  "    return",
  ' f"Hello, {name}!"',
  "\n",
  'print(greet("World"))',
  "\n",
  "```",
  "\n\n",
  "## Math",
  "\n\n",
  "Inline",
  " math:",
  " $a^2",
  " +",
  " b^2",
  " =",
  " c^2$",
  ".",
  "\n\n",
  "Displayed",
  " equation:",
  "\n\n",
  "$$",
  "\n",
  "\\int_0^1",
  " x^2",
  " dx",
  " =",
  " \\frac{1}{3}",
  "\n",
  "$$",
  "\n\n",
]

const Example = () => {
  const [content, setContent] = useState("")

  useEffect(() => {
    let currentContent = ""
    let index = 0

    const interval = setInterval(() => {
      if (index < tokens.length) {
        currentContent += tokens[index]
        setContent(currentContent)
        index++
      } else {
        clearInterval(interval)
      }
    }, 100)

    return () => clearInterval(interval)
  }, [])

  return (
    <div className="h-full min-h-0 w-full overflow-hidden">
      <Response className="h-full overflow-auto p-10">{content}</Response>
    </div>
  )
}

export default Example
```

#### Component Source Code

##### File: `components/ui/response.tsx`

```tsx
"use client"

import { memo, type ComponentProps } from "react"
import { Streamdown } from "streamdown"

import { cn } from "@/lib/utils"

type ResponseProps = ComponentProps<typeof Streamdown>

export const Response = memo(
  ({ className, ...props }: ResponseProps) => (
    <Streamdown
      className={cn(
        "size-full [&>*:first-child]:mt-0 [&>*:last-child]:mb-0",
        className
      )}
      {...props}
    />
  ),
  (prevProps, nextProps) => prevProps.children === nextProps.children
)

Response.displayName = "Response"
```

---

### Scrub Bar <a name="scrub-bar"></a>

A component for scrubbing through a timeline, typically used for audio or video playback.

- **Registry Key**: `scrub-bar`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/scrub-bar.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `progress`

#### Demo Code

##### File: `examples/scrub-bar-demo.tsx`

```tsx
"use client"

import * as React from "react"

import {
  ScrubBarContainer,
  ScrubBarProgress,
  ScrubBarThumb,
  ScrubBarTimeLabel,
  ScrubBarTrack,
} from "@/components/ui/scrub-bar"

const ScrubBarDemo = () => {
  const [value, setValue] = React.useState(30)
  const [isScrubbing, setIsScrubbing] = React.useState(false)
  const duration = 100

  return (
    <div className="flex w-full max-w-sm flex-col items-center gap-4 p-4">
      <ScrubBarContainer
        duration={duration}
        value={value}
        onScrub={setValue}
        onScrubStart={() => setIsScrubbing(true)}
        onScrubEnd={() => setIsScrubbing(false)}
        className="w-full"
      >
        <ScrubBarTimeLabel time={value} className="w-10 text-center" />
        <ScrubBarTrack className="mx-2">
          <ScrubBarProgress />
          <ScrubBarThumb className="bg-primary" data-scrubbing={isScrubbing} />
        </ScrubBarTrack>
        <ScrubBarTimeLabel time={duration} className="w-10 text-center" />
      </ScrubBarContainer>
    </div>
  )
}

export default ScrubBarDemo
```

#### Component Source Code

##### File: `components/ui/scrub-bar.tsx`

```tsx
"use client"

import * as React from "react"
import {
  createContext,
  useCallback,
  useContext,
  useRef,
  type ComponentProps,
  type HTMLAttributes,
} from "react"

import { cn } from "@/lib/utils"
import { Progress } from "@/components/ui/progress"

function formatTimestamp(value: number) {
  if (!Number.isFinite(value) || value < 0) return "0:00"
  const totalSeconds = Math.floor(value)
  const minutes = Math.floor(totalSeconds / 60)
  const seconds = totalSeconds % 60
  return `${minutes}:${seconds.toString().padStart(2, "0")}`
}

interface ScrubBarContextValue {
  duration: number
  value: number
  progress: number
  onScrub?: (time: number) => void
  onScrubStart?: () => void
  onScrubEnd?: () => void
}

const ScrubBarContext = createContext<ScrubBarContextValue | null>(null)

function useScrubBarContext() {
  const context = useContext(ScrubBarContext)
  if (!context) {
    throw new Error("useScrubBarContext must be used within a ScrubBar.Root")
  }
  return context
}

interface ScrubBarContainerProps extends HTMLAttributes<HTMLDivElement> {
  duration: number
  value: number
  onScrub?: (time: number) => void
  onScrubStart?: () => void
  onScrubEnd?: () => void
}

function ScrubBarContainer({
  duration,
  value,
  onScrub,
  onScrubStart,
  onScrubEnd,
  children,
  className,
  ...props
}: ScrubBarContainerProps) {
  const progress = duration > 0 ? (value / duration) * 100 : 0

  const contextValue: ScrubBarContextValue = {
    duration,
    value,
    progress,
    onScrub,
    onScrubStart,
    onScrubEnd,
  }

  return (
    <ScrubBarContext.Provider value={contextValue}>
      <div
        data-slot="scrub-bar-root"
        className={cn("flex w-full items-center", className)}
        {...props}
      >
        {children}
      </div>
    </ScrubBarContext.Provider>
  )
}
ScrubBarContainer.displayName = "ScrubBarContainer"

type ScrubBarTrackProps = HTMLAttributes<HTMLDivElement>

function ScrubBarTrack({ className, children, ...props }: ScrubBarTrackProps) {
  const trackRef = useRef<HTMLDivElement | null>(null)
  const { duration, onScrub, onScrubStart, onScrubEnd, value } =
    useScrubBarContext()

  const getTimeFromClientX = useCallback(
    (clientX: number) => {
      const track = trackRef.current
      if (!track || !duration) return null
      const rect = track.getBoundingClientRect()
      const ratio = (clientX - rect.left) / rect.width
      const clamped = Math.min(Math.max(ratio, 0), 1)
      return duration * clamped
    },
    [duration]
  )

  const handlePointerDown = useCallback(
    (event: React.PointerEvent<HTMLDivElement>) => {
      if (!duration) return
      event.preventDefault()
      onScrubStart?.()
      const time = getTimeFromClientX(event.clientX)
      if (time != null) {
        onScrub?.(time)
      }

      const handleMove = (moveEvent: PointerEvent) => {
        const nextTime = getTimeFromClientX(moveEvent.clientX)
        if (nextTime != null) {
          onScrub?.(nextTime)
        }
      }

      const handleUp = () => {
        onScrubEnd?.()
        window.removeEventListener("pointermove", handleMove)
        window.removeEventListener("pointerup", handleUp)
      }

      window.addEventListener("pointermove", handleMove)
      window.addEventListener("pointerup", handleUp, { once: true })
    },
    [duration, getTimeFromClientX, onScrub, onScrubEnd, onScrubStart]
  )

  const clampedValue = Math.min(Math.max(value, 0), duration || 0)

  return (
    <div
      ref={trackRef}
      data-slot="scrub-bar-track"
      className={cn(
        "bg-secondary relative h-2 w-full grow cursor-pointer touch-none rounded-full transition-none select-none",
        className
      )}
      onPointerDown={handlePointerDown}
      role="slider"
      aria-valuemin={0}
      aria-valuemax={duration || 0}
      aria-valuenow={clampedValue}
      {...props}
    >
      {children}
    </div>
  )
}
ScrubBarTrack.displayName = "ScrubBarTrack"

type ScrubBarProgressProps = Omit<ComponentProps<typeof Progress>, "value">

function ScrubBarProgress({ className, ...props }: ScrubBarProgressProps) {
  const { progress } = useScrubBarContext()

  return (
    <Progress
      data-slot="scrub-bar-progress"
      value={progress}
      className={cn("absolute h-full [&>div]:transition-none", className)}
      {...props}
    />
  )
}
ScrubBarProgress.displayName = "ScrubBarProgress"

type ScrubBarThumbProps = HTMLAttributes<HTMLDivElement>

function ScrubBarThumb({ className, children, ...props }: ScrubBarThumbProps) {
  const { progress } = useScrubBarContext()
  return (
    <div
      data-slot="scrub-bar-thumb"
      className={cn(
        "bg-primary absolute top-1/2 block h-4 w-4 -translate-x-1/2 -translate-y-1/2 rounded-full transition-colors disabled:pointer-events-none disabled:opacity-50",
        className
      )}
      style={{ left: `${progress}%` }}
      {...props}
    >
      {children}
    </div>
  )
}
ScrubBarThumb.displayName = "ScrubBarThumb"

interface ScrubBarTimeLabelProps extends HTMLAttributes<HTMLSpanElement> {
  time: number
  format?: (time: number) => string
}

function ScrubBarTimeLabel({
  className,
  time,
  format = formatTimestamp,
  ...props
}: ScrubBarTimeLabelProps) {
  return (
    <span
      data-slot="scrub-bar-time-label"
      {...props}
      className={cn("tabular-nums", className)}
    >
      {format(time)}
    </span>
  )
}
ScrubBarTimeLabel.displayName = "ScrubBarTimeLabel"

export {
  ScrubBarContainer,
  ScrubBarTrack,
  ScrubBarProgress,
  ScrubBarThumb,
  ScrubBarTimeLabel,
}
```

---

### Shimmering Text <a name="shimmering-text"></a>

Animated text with gradient shimmer effects and viewport-triggered animations using Motion.

- **Registry Key**: `shimmering-text`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/shimmering-text.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`

#### Demo Code

##### File: `examples/shimmering-text-demo.tsx`

```tsx
"use client"

import { useEffect, useState } from "react"
import { AnimatePresence, motion } from "motion/react"

import { ShimmeringText } from "@/components/ui/shimmering-text"

const phrases = [
  "Agent is thinking...",
  "Processing your request...",
  "Analyzing the data...",
  "Generating response...",
  "Almost there...",
]

export default function TextShimmerDemo() {
  const [currentIndex, setCurrentIndex] = useState(0)

  useEffect(() => {
    const interval = setInterval(() => {
      setCurrentIndex((prev) => (prev + 1) % phrases.length)
    }, 3000)

    return () => clearInterval(interval)
  }, [])

  return (
    <div className="bg-card w-full rounded-lg border p-6">
      <div className="mb-4">
        <h3 className="text-lg font-semibold">Text Shimmer Effect</h3>
        <p className="text-muted-foreground text-sm">
          Animated gradient text with automatic cycling
        </p>
      </div>

      <div className="space-y-4">
        <div className="bg-muted/10 flex items-center justify-center rounded-lg py-8">
          <AnimatePresence mode="wait">
            <motion.div
              key={currentIndex}
              initial={{ opacity: 0, y: 10 }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, y: -10 }}
              transition={{ duration: 0.3 }}
            >
              <ShimmeringText text={phrases[currentIndex]} />
            </motion.div>
          </AnimatePresence>
        </div>
      </div>
    </div>
  )
}
```

#### Props & API Reference

##### `Props` Props

| Prop         | Type      | Default | Description                                          |
| ------------ | --------- | ------- | ---------------------------------------------------- |
| text         | `string`  | -       | **Required.** Text to display with shimmer effect    |
| duration     | `number`  | `2`     | Animation duration in seconds                        |
| delay        | `number`  | `0`     | Delay before starting animation                      |
| repeat       | `boolean` | `true`  | Whether to repeat the animation                      |
| repeatDelay  | `number`  | `0.5`   | Pause duration between repeats in seconds            |
| className    | `string`  | -       | Optional CSS classes                                 |
| startOnView  | `boolean` | `true`  | Whether to start animation when entering viewport    |
| once         | `boolean` | `false` | Whether to animate only once                         |
| inViewMargin | `string`  | -       | Margin for viewport detection (e.g., "0px 0px -10%") |
| spread       | `number`  | `2`     | Shimmer spread multiplier                            |
| color        | `string`  | -       | Base text color (CSS custom property)                |
| shimmerColor | `string`  | -       | Shimmer gradient color (CSS custom property)         |

#### Component Source Code

##### File: `components/ui/shimmering-text.tsx`

```tsx
"use client"

import React, { useMemo, useRef } from "react"
import { motion, useInView, UseInViewOptions } from "motion/react"

import { cn } from "@/lib/utils"

interface ShimmeringTextProps {
  /** Text to display with shimmer effect */
  text: string
  /** Animation duration in seconds */
  duration?: number
  /** Delay before starting animation */
  delay?: number
  /** Whether to repeat the animation */
  repeat?: boolean
  /** Pause duration between repeats in seconds */
  repeatDelay?: number
  /** Custom className */
  className?: string
  /** Whether to start animation when component enters viewport */
  startOnView?: boolean
  /** Whether to animate only once */
  once?: boolean
  /** Margin for in-view detection (rootMargin) */
  inViewMargin?: UseInViewOptions["margin"]
  /** Shimmer spread multiplier */
  spread?: number
  /** Base text color */
  color?: string
  /** Shimmer gradient color */
  shimmerColor?: string
}

export function ShimmeringText({
  text,
  duration = 2,
  delay = 0,
  repeat = true,
  repeatDelay = 0.5,
  className,
  startOnView = true,
  once = false,
  inViewMargin,
  spread = 2,
  color,
  shimmerColor,
}: ShimmeringTextProps) {
  const ref = useRef<HTMLSpanElement>(null)
  const isInView = useInView(ref, { once, margin: inViewMargin })

  // Calculate dynamic spread based on text length
  const dynamicSpread = useMemo(() => {
    return text.length * spread
  }, [text, spread])

  // Determine if we should start animation
  const shouldAnimate = !startOnView || isInView

  return (
    <motion.span
      ref={ref}
      className={cn(
        "relative inline-block bg-[length:250%_100%,auto] bg-clip-text text-transparent",
        "[--base-color:var(--muted-foreground)] [--shimmer-color:var(--foreground)]",
        "[background-repeat:no-repeat,padding-box]",
        "[--shimmer-bg:linear-gradient(90deg,transparent_calc(50%-var(--spread)),var(--shimmer-color),transparent_calc(50%+var(--spread)))]",
        "dark:[--base-color:var(--muted-foreground)] dark:[--shimmer-color:var(--foreground)]",
        className
      )}
      style={
        {
          "--spread": `${dynamicSpread}px`,
          ...(color && { "--base-color": color }),
          ...(shimmerColor && { "--shimmer-color": shimmerColor }),
          backgroundImage: `var(--shimmer-bg), linear-gradient(var(--base-color), var(--base-color))`,
        } as React.CSSProperties
      }
      initial={{
        backgroundPosition: "100% center",
        opacity: 0,
      }}
      animate={
        shouldAnimate
          ? {
              backgroundPosition: "0% center",
              opacity: 1,
            }
          : {}
      }
      transition={{
        backgroundPosition: {
          repeat: repeat ? Infinity : 0,
          duration,
          delay,
          repeatDelay,
          ease: "linear",
        },
        opacity: {
          duration: 0.3,
          delay,
        },
      }}
    >
      {text}
    </motion.span>
  )
}
```

---

### Speech Input <a name="speech-input"></a>

A compact speech-to-text input component with real-time transcription using ElevenLabs Scribe.

- **Registry Key**: `speech-input`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/speech-input.json
```

#### Dependencies

- **NPM Packages**:
  - `motion`
  - `lucide-react`
- **Local Registry Dependencies**:
  - `button`
  - `skeleton`
  - `https://ui.elevenlabs.io/r/use-scribe.json`

#### How to Use

##### Example 1

```tsx
{
  echoCancellation: true,
  noiseSuppression: true
}
```

##### Example 2

```tsx
interface SpeechInputData {
  partialTranscript: string
  committedTranscripts: string[]
  transcript: string // Combined full transcript
}
```

##### Example 3

```tsx
enum CommitStrategy {
  MANUAL = "manual",
  VAD = "vad",
}
```

##### Example 4

```tsx
enum AudioFormat {
  PCM_8000 = "pcm_8000",
  PCM_16000 = "pcm_16000",
  PCM_22050 = "pcm_22050",
  PCM_24000 = "pcm_24000",
  PCM_44100 = "pcm_44100",
  PCM_48000 = "pcm_48000",
  ULAW_8000 = "ulaw_8000",
}
```

#### Demo Code

##### File: `examples/speech-input-demo.tsx`

```tsx
"use client"

import { useRef, useState } from "react"
import { toast } from "sonner"

import { getScribeToken } from "@/registry/elevenlabs-ui/blocks/realtime-transcriber-01/actions/get-scribe-token"
import { Input } from "@/components/ui/input"
import {
  SpeechInput,
  SpeechInputCancelButton,
  SpeechInputPreview,
  SpeechInputRecordButton,
} from "@/components/ui/speech-input"
import { Textarea } from "@/components/ui/textarea"

async function getToken() {
  const result = await getScribeToken()
  if (result.error) {
    throw new Error(result.error)
  }
  return result.token!
}

function TextareaWithSpeechInputRight() {
  const [value, setValue] = useState("")
  const valueAtStartRef = useRef("")

  return (
    <div className="relative">
      <Textarea
        value={value}
        onChange={(event) => {
          setValue(event.target.value)
        }}
        placeholder="Jot down some thoughts..."
        className="min-h-[120px] resize-none rounded-2xl px-3.5 pt-3 pb-14"
      />
      <div className="absolute right-3 bottom-3 flex items-center gap-2">
        <SpeechInput
          size="sm"
          getToken={getToken}
          onStart={() => {
            valueAtStartRef.current = value
          }}
          onChange={({ transcript }) => {
            setValue(valueAtStartRef.current + transcript)
          }}
          onStop={({ transcript }) => {
            setValue(valueAtStartRef.current + transcript)
          }}
          onCancel={() => {
            setValue(valueAtStartRef.current)
          }}
          onError={(error) => {
            toast.error(String(error))
          }}
        >
          <SpeechInputCancelButton />
          <SpeechInputPreview placeholder="Listening..." />
          <SpeechInputRecordButton />
        </SpeechInput>
      </div>
    </div>
  )
}

function TextareaWithSpeechInputLeft() {
  const [value, setValue] = useState("")
  const valueAtStartRef = useRef("")

  return (
    <div className="relative">
      <Textarea
        value={value}
        onChange={(event) => {
          setValue(event.target.value)
        }}
        placeholder="Jot down some thoughts..."
        className="min-h-[120px] resize-none rounded-2xl px-3.5 pt-3 pb-14"
      />
      <div className="absolute bottom-3 left-3 flex items-center gap-2">
        <SpeechInput
          size="sm"
          getToken={getToken}
          onStart={() => {
            valueAtStartRef.current = value
          }}
          onChange={({ transcript }) => {
            setValue(valueAtStartRef.current + transcript)
          }}
          onStop={({ transcript }) => {
            setValue(valueAtStartRef.current + transcript)
          }}
          onCancel={() => {
            setValue(valueAtStartRef.current)
          }}
          onError={(error) => {
            toast.error(String(error))
          }}
        >
          <SpeechInputRecordButton />
          <SpeechInputPreview placeholder="Listening..." />
          <SpeechInputCancelButton />
        </SpeechInput>
      </div>
    </div>
  )
}

function InputWithSpeechInput() {
  const [value, setValue] = useState("")
  const valueAtStartRef = useRef("")

  return (
    <div className="flex items-center gap-2.5">
      <Input
        value={value}
        onChange={(event) => {
          setValue(event.target.value)
        }}
        placeholder="Give this idea a title..."
        className="min-w-0 flex-1 px-3.5 text-base transition-[flex-basis] duration-200 md:text-sm"
      />
      <SpeechInput
        getToken={getToken}
        className="shrink-0"
        onStart={() => {
          valueAtStartRef.current = value
        }}
        onChange={({ transcript }) => {
          setValue(valueAtStartRef.current + transcript)
        }}
        onStop={({ transcript }) => {
          setValue(valueAtStartRef.current + transcript)
        }}
        onCancel={() => {
          setValue(valueAtStartRef.current)
        }}
        onError={(error) => {
          toast.error(String(error))
        }}
      >
        <SpeechInputCancelButton />
        <SpeechInputRecordButton />
      </SpeechInput>
    </div>
  )
}

export default function SpeechInputDemo() {
  return (
    <div className="absolute inset-0 space-y-4 overflow-auto rounded-2xl p-10">
      <TextareaWithSpeechInputRight />
      <TextareaWithSpeechInputLeft />
      <InputWithSpeechInput />
    </div>
  )
}
```

#### Props & API Reference

##### `Props` Props

| Prop        | Type                              | Default          | Description           |
| ----------- | --------------------------------- | ---------------- | --------------------- |
| placeholder | `string`                          | `"Listening..."` | Text shown when empty |
| className   | `string`                          | -                | Optional CSS classes  |
| ...props    | `ComponentPropsWithoutRef<"div">` | -                | All div props         |

#### Component Source Code

##### File: `components/ui/speech-input.tsx`

```tsx
"use client"

import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { motion } from "framer-motion"
import { MicIcon, SquareIcon, XIcon } from "lucide-react"

import { cn } from "@/lib/utils"
import {
  useScribe,
  type AudioFormat,
  type CommitStrategy,
} from "@/registry/elevenlabs-ui/hooks/use-scribe"
import { Button } from "@/components/ui/button"
import { Skeleton } from "@/components/ui/skeleton"

const buttonVariants = cva("!px-0", {
  variants: {
    size: {
      default: "h-9 w-9",
      sm: "h-8 w-8",
      lg: "h-10 w-10",
    },
  },
  defaultVariants: {
    size: "default",
  },
})

type ButtonSize = VariantProps<typeof buttonVariants>["size"]

export interface SpeechInputData {
  /** The current partial (in-progress) transcript */
  partialTranscript: string
  /** Array of all committed (finalized) transcripts */
  committedTranscripts: string[]
  /** Full transcript combining committed and partial transcripts */
  transcript: string
}

interface SpeechInputContextValue {
  isConnected: boolean
  isConnecting: boolean
  transcript: string
  partialTranscript: string
  committedTranscripts: string[]
  error: string | null
  start: () => Promise<void>
  stop: () => void
  cancel: () => void
  size: ButtonSize
}

const SpeechInputContext = React.createContext<SpeechInputContextValue | null>(
  null
)

function useSpeechInput() {
  const context = React.useContext(SpeechInputContext)
  if (!context) {
    throw new Error(
      "SpeechInput compound components must be used within a SpeechInput"
    )
  }
  return context
}

function buildTranscript({
  partialTranscript,
  committedTranscripts,
}: {
  partialTranscript: string
  committedTranscripts: string[]
}): string {
  const committed = committedTranscripts.join(" ").trim()
  const partial = partialTranscript.trim()

  if (committed && partial) {
    return `${committed} ${partial}`
  }
  return committed || partial
}

function buildData({
  partialTranscript,
  committedTranscripts,
}: {
  partialTranscript: string
  committedTranscripts: string[]
}): SpeechInputData {
  return {
    partialTranscript,
    committedTranscripts,
    transcript: buildTranscript({ partialTranscript, committedTranscripts }),
  }
}

export interface SpeechInputProps {
  children: React.ReactNode

  /**
   * Function that returns a token for authenticating with the speech service.
   * This should be an async function that fetches a token from your backend.
   */
  getToken: () => Promise<string>

  /**
   * Called whenever the transcript changes (partial or committed)
   */
  onChange?: (data: SpeechInputData) => void

  /**
   * Called when recording is cancelled
   */
  onCancel?: (data: SpeechInputData) => void

  /**
   * Called when recording starts
   */
  onStart?: (data: SpeechInputData) => void

  /**
   * Called when recording stops
   */
  onStop?: (data: SpeechInputData) => void

  /**
   * Additional CSS classes for the root container
   */
  className?: string

  /**
   * Size variant for the component buttons
   * @default "default"
   */
  size?: ButtonSize

  /**
   * Model ID for the speech recognition service
   * @default "scribe_v2_realtime"
   */
  modelId?: string

  /**
   * Base URI for the speech recognition service
   */
  baseUri?: string

  /**
   * Strategy for committing transcripts
   */
  commitStrategy?: CommitStrategy

  /**
   * Silence threshold in seconds for VAD
   */
  vadSilenceThresholdSecs?: number

  /**
   * VAD threshold value
   */
  vadThreshold?: number

  /**
   * Minimum speech duration in milliseconds
   */
  minSpeechDurationMs?: number

  /**
   * Minimum silence duration in milliseconds
   */
  minSilenceDurationMs?: number

  /**
   * Language code for transcription (e.g., "en", "es", "fr")
   */
  languageCode?: string

  /**
   * Microphone configuration options
   */
  microphone?: {
    deviceId?: string
    echoCancellation?: boolean
    noiseSuppression?: boolean
    autoGainControl?: boolean
    channelCount?: number
  }

  /**
   * Audio format for manual audio mode
   */
  audioFormat?: AudioFormat

  /**
   * Sample rate for manual audio mode
   */
  sampleRate?: number

  /**
   * Called when an error occurs
   */
  onError?: (error: Error | Event) => void

  /**
   * Called when an authentication error occurs
   */
  onAuthError?: (data: { error: string }) => void

  /**
   * Called when a quota exceeded error occurs
   */
  onQuotaExceededError?: (data: { error: string }) => void
}

const SpeechInput = React.forwardRef<HTMLDivElement, SpeechInputProps>(
  function SpeechInput(
    {
      children,
      getToken,
      onChange,
      onCancel,
      onStart,
      onStop,
      className,
      size = "default",
      modelId = "scribe_v2_realtime",
      baseUri,
      commitStrategy,
      vadSilenceThresholdSecs,
      vadThreshold,
      minSpeechDurationMs,
      minSilenceDurationMs,
      languageCode,
      microphone = {
        echoCancellation: true,
        noiseSuppression: true,
      },
      audioFormat,
      sampleRate,
      onError,
      onAuthError,
      onQuotaExceededError,
    },
    ref
  ) {
    const transcriptsRef = React.useRef({
      partialTranscript: "",
      committedTranscripts: [] as string[],
    })
    const startRequestIdRef = React.useRef(0)

    const scribe = useScribe({
      modelId,
      baseUri,
      commitStrategy,
      vadSilenceThresholdSecs,
      vadThreshold,
      minSpeechDurationMs,
      minSilenceDurationMs,
      languageCode,
      audioFormat,
      sampleRate,
      microphone,
      onPartialTranscript: (data) => {
        transcriptsRef.current.partialTranscript = data.text
        onChange?.(buildData(transcriptsRef.current))
      },
      onCommittedTranscript: (data) => {
        transcriptsRef.current.committedTranscripts.push(data.text)
        transcriptsRef.current.partialTranscript = ""
        onChange?.(buildData(transcriptsRef.current))
      },
      onError,
      onAuthError,
      onQuotaExceededError,
    })

    const isConnecting = scribe.status === "connecting"

    const start = React.useCallback(async () => {
      const requestId = startRequestIdRef.current + 1
      startRequestIdRef.current = requestId

      transcriptsRef.current = {
        partialTranscript: "",
        committedTranscripts: [],
      }
      scribe.clearTranscripts()

      try {
        const token = await getToken()
        if (startRequestIdRef.current !== requestId) {
          return
        }

        await scribe.connect({
          token,
        })
        if (startRequestIdRef.current !== requestId) {
          scribe.disconnect()
          return
        }
        onStart?.(buildData(transcriptsRef.current))
      } catch (error) {
        onError?.(error instanceof Error ? error : new Error(String(error)))
      }
    }, [getToken, scribe, onStart, onError])

    const stop = React.useCallback(() => {
      startRequestIdRef.current += 1
      scribe.disconnect()
      onStop?.(buildData(transcriptsRef.current))
    }, [scribe, onStop])

    const cancel = React.useCallback(() => {
      startRequestIdRef.current += 1
      const data = buildData(transcriptsRef.current)
      scribe.disconnect()
      scribe.clearTranscripts()
      transcriptsRef.current = {
        partialTranscript: "",
        committedTranscripts: [],
      }
      onCancel?.(data)
    }, [scribe, onCancel])

    const contextValue: SpeechInputContextValue = React.useMemo(
      () => ({
        isConnected: scribe.isConnected,
        isConnecting,
        start,
        stop,
        cancel,
        error: scribe.error,
        size,
        ...buildData({
          partialTranscript: scribe.partialTranscript,
          committedTranscripts: scribe.committedTranscripts.map((t) => t.text),
        }),
      }),
      [
        scribe.isConnected,
        scribe.error,
        scribe.partialTranscript,
        scribe.committedTranscripts,
        isConnecting,
        start,
        stop,
        cancel,
        size,
      ]
    )

    React.useEffect(() => {
      return () => {
        startRequestIdRef.current += 1
        scribe.disconnect()
      }
    }, [scribe.disconnect])

    return (
      <SpeechInputContext.Provider value={contextValue}>
        <div
          ref={ref}
          className={cn(
            "relative inline-flex items-center overflow-hidden rounded-md transition-all duration-200",
            scribe.isConnected
              ? "bg-background dark:bg-muted shadow-[inset_0_0_0_1px_var(--color-input),0_1px_2px_0_rgba(0,0,0,0.05)]"
              : "",
            className
          )}
        >
          {children}
        </div>
      </SpeechInputContext.Provider>
    )
  }
)

SpeechInput.displayName = "SpeechInput"

export type SpeechInputRecordButtonProps = Omit<
  React.ComponentPropsWithoutRef<typeof Button>,
  "size"
>

/**
 * Toggle button for starting/stopping speech recording.
 * Shows a microphone icon when idle and a stop icon when recording.
 */
const SpeechInputRecordButton = React.forwardRef<
  HTMLButtonElement,
  SpeechInputRecordButtonProps
>(function SpeechInputRecordButton(
  { className, onClick, variant = "ghost", disabled, ...props },
  ref
) {
  const speechInput = useSpeechInput()

  return (
    <Button
      ref={ref}
      type="button"
      variant={variant}
      onClick={(e) => {
        if (speechInput.isConnected) {
          speechInput.stop()
        } else {
          speechInput.start()
        }
        onClick?.(e)
      }}
      disabled={disabled ?? speechInput.isConnecting}
      className={cn(
        buttonVariants({ size: speechInput.size }),
        "relative flex items-center justify-center transition-all",
        speechInput.isConnected && "scale-[80%]",
        className
      )}
      aria-label={
        speechInput.isConnected ? "Stop recording" : "Start recording"
      }
      {...props}
    >
      <Skeleton
        className={cn(
          "absolute h-4 w-4 rounded-full transition-all duration-200",
          speechInput.isConnecting
            ? "bg-primary scale-90"
            : "scale-[60%] bg-transparent"
        )}
      />
      <SquareIcon
        className={cn(
          "text-destructive absolute h-4 w-4 fill-current transition-all duration-200",
          !speechInput.isConnecting && speechInput.isConnected
            ? "scale-100 opacity-100"
            : "scale-[60%] opacity-0"
        )}
      />
      <MicIcon
        className={cn(
          "absolute h-4 w-4 transition-all duration-200",
          !speechInput.isConnecting && !speechInput.isConnected
            ? "scale-100 opacity-100"
            : "scale-[60%] opacity-0"
        )}
      />
    </Button>
  )
})

SpeechInputRecordButton.displayName = "SpeechInputRecordButton"

export interface SpeechInputPreviewProps
  extends React.ComponentPropsWithoutRef<"div"> {
  /**
   * Text to show when no transcript is available
   * @default "Listening..."
   */
  placeholder?: string
}

/**
 * Displays the current transcript with a placeholder when empty.
 * Only visible when actively recording.
 */
const SpeechInputPreview = React.forwardRef<
  HTMLDivElement,
  SpeechInputPreviewProps
>(function SpeechInputPreview(
  { className, placeholder = "Listening...", ...props },
  ref
) {
  const speechInput = useSpeechInput()

  const displayText = speechInput.transcript || placeholder
  const showPlaceholder = !speechInput.transcript.trim()

  return (
    <div
      ref={ref}
      inert={speechInput.isConnected ? undefined : true}
      className={cn(
        "relative self-stretch text-sm transition-[opacity,transform,width] duration-200 ease-out",
        showPlaceholder
          ? "text-muted-foreground italic"
          : "text-muted-foreground",
        speechInput.isConnected ? "w-28 opacity-100" : "w-0 opacity-0",
        className
      )}
      title={displayText}
      aria-hidden={!speechInput.isConnected}
      {...props}
    >
      <div className="absolute inset-y-0 -right-1 -left-1 [mask-image:linear-gradient(to_right,transparent,black_10px,black_calc(100%-10px),transparent)]">
        <motion.p
          key="text"
          layout="position"
          className="absolute top-0 right-0 bottom-0 flex h-full min-w-full items-center px-1 whitespace-nowrap"
        >
          {displayText}
        </motion.p>
      </div>
    </div>
  )
})

SpeechInputPreview.displayName = "SpeechInputPreview"

export type SpeechInputCancelButtonProps = Omit<
  React.ComponentPropsWithoutRef<typeof Button>,
  "size"
>

/**
 * Button to cancel the current recording and discard the transcript.
 * Only visible when actively recording.
 */
const SpeechInputCancelButton = React.forwardRef<
  HTMLButtonElement,
  SpeechInputCancelButtonProps
>(function SpeechInputCancelButton(
  { className, onClick, variant = "ghost", ...props },
  ref
) {
  const speechInput = useSpeechInput()

  return (
    <Button
      ref={ref}
      type="button"
      variant={variant}
      inert={speechInput.isConnected ? undefined : true}
      onClick={(e) => {
        speechInput.cancel()
        onClick?.(e)
      }}
      className={cn(
        buttonVariants({ size: speechInput.size }),
        "transition-[opacity,transform,width] duration-200 ease-out",
        speechInput.isConnected
          ? "scale-[80%] opacity-100"
          : "pointer-events-none w-0 scale-100 opacity-0",
        className
      )}
      aria-label="Cancel recording"
      {...props}
    >
      <XIcon className="h-3 w-3" />
    </Button>
  )
})

SpeechInputCancelButton.displayName = "SpeechInputCancelButton"

export {
  SpeechInput,
  SpeechInputRecordButton,
  SpeechInputPreview,
  SpeechInputCancelButton,
  useSpeechInput,
}
```

---

### Transcript Viewer <a name="transcript-viewer"></a>

A component for displaying an audio transcript with word-by-word highlighting synced to audio playback.

- **Registry Key**: `transcript-viewer`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/transcript-viewer.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `button`
  - `https://ui.elevenlabs.io/r/scrub-bar.json`

#### Demo Code

##### File: `examples/transcript-viewer-demo.tsx`

```tsx
"use client"

import { useEffect, useState } from "react"
import { PauseIcon, PlayIcon } from "lucide-react"

import {
  TranscriptViewerAudio,
  TranscriptViewerContainer,
  TranscriptViewerPlayPauseButton,
  TranscriptViewerScrubBar,
  TranscriptViewerWords,
  type CharacterAlignmentResponseModel,
} from "@/components/ui/transcript-viewer"

import { Skeleton } from "../ui/skeleton"

const TranscriptViewerDemo = () => {
  const audioSrc = "/sounds/transcript-viewer/transcript-viewer-audio.mp3"
  const [alignment, setAlignment] = useState<
    CharacterAlignmentResponseModel | undefined
  >(undefined)

  useEffect(() => {
    fetch("/sounds/transcript-viewer/transcript-viewer-alignment.json")
      .then((res) => res.json())
      .then((data) => setAlignment(data))
  }, [])

  return (
    <div className="flex w-full flex-col gap-4">
      <TranscriptViewerContainer
        key={audioSrc}
        className="bg-card w-full rounded-xl border p-4"
        audioSrc={audioSrc}
        audioType="audio/mpeg"
        alignment={
          alignment ?? {
            characters: [],
            characterStartTimesSeconds: [],
            characterEndTimesSeconds: [],
          }
        }
      >
        <TranscriptViewerAudio className="sr-only" />
        {alignment ? (
          <>
            <TranscriptViewerWords />
            <div className="flex items-center gap-3">
              <TranscriptViewerScrubBar />
            </div>
          </>
        ) : (
          <div className="flex w-full flex-col gap-3">
            <Skeleton className="h-5 w-full" />
            <Skeleton className="mb-4 h-5 w-1/2" />
            <Skeleton className="h-2 w-full" />
            <div className="-mt-1 flex items-center justify-between">
              <Skeleton className="h-2 w-6" />
              <Skeleton className="h-2 w-6" />
            </div>
          </div>
        )}
        <TranscriptViewerPlayPauseButton
          className="w-full cursor-pointer"
          size="default"
          disabled={!alignment}
        >
          {({ isPlaying }) => (
            <>
              {isPlaying ? (
                <>
                  <PauseIcon className="size-4" /> Pause
                </>
              ) : (
                <>
                  <PlayIcon className="size-4" /> Play
                </>
              )}
            </>
          )}
        </TranscriptViewerPlayPauseButton>
      </TranscriptViewerContainer>
    </div>
  )
}
export default TranscriptViewerDemo
```

#### Component Source Code

##### File: `components/ui/transcript-viewer.tsx`

```tsx
"use client"

import {
  createContext,
  useContext,
  useMemo,
  type ComponentPropsWithoutRef,
  type ComponentPropsWithRef,
  type HTMLAttributes,
  type ReactNode,
} from "react"
import type { CharacterAlignmentResponseModel } from "@elevenlabs/elevenlabs-js/api/types/CharacterAlignmentResponseModel"
import { Pause, Play } from "lucide-react"

import { cn } from "@/lib/utils"
import {
  useTranscriptViewer,
  type SegmentComposer,
  type TranscriptSegment,
  type TranscriptWord as TranscriptWordType,
  type UseTranscriptViewerResult,
} from "@/registry/elevenlabs-ui/hooks/use-transcript-viewer"
import { Button } from "@/components/ui/button"
import {
  ScrubBarContainer,
  ScrubBarProgress,
  ScrubBarThumb,
  ScrubBarTimeLabel,
  ScrubBarTrack,
} from "@/components/ui/scrub-bar"

type TranscriptGap = Extract<TranscriptSegment, { kind: "gap" }>

type TranscriptViewerContextValue = UseTranscriptViewerResult & {
  audioProps: Omit<ComponentPropsWithRef<"audio">, "children" | "src">
}

const TranscriptViewerContext =
  createContext<TranscriptViewerContextValue | null>(null)

function useTranscriptViewerContext() {
  const context = useContext(TranscriptViewerContext)
  if (!context) {
    throw new Error(
      "useTranscriptViewerContext must be used within a TranscriptViewer"
    )
  }
  return context
}

type TranscriptViewerProviderProps = {
  value: TranscriptViewerContextValue
  children: ReactNode
}

function TranscriptViewerProvider({
  value,
  children,
}: TranscriptViewerProviderProps) {
  return (
    <TranscriptViewerContext.Provider value={value}>
      {children}
    </TranscriptViewerContext.Provider>
  )
}

type AudioType =
  | "audio/mpeg"
  | "audio/wav"
  | "audio/ogg"
  | "audio/mp3"
  | "audio/m4a"
  | "audio/aac"
  | "audio/webm"

type TranscriptViewerContainerProps = {
  audioSrc: string
  audioType: AudioType
  alignment: CharacterAlignmentResponseModel
  segmentComposer?: SegmentComposer
  hideAudioTags?: boolean
  children?: ReactNode
} & Omit<ComponentPropsWithoutRef<"div">, "children"> &
  Pick<
    Parameters<typeof useTranscriptViewer>[0],
    "onPlay" | "onPause" | "onTimeUpdate" | "onEnded" | "onDurationChange"
  >

function TranscriptViewerContainer({
  audioSrc,
  audioType = "audio/mpeg",
  alignment,
  segmentComposer,
  hideAudioTags = true,
  children,
  className,
  onPlay,
  onPause,
  onTimeUpdate,
  onEnded,
  onDurationChange,
  ...props
}: TranscriptViewerContainerProps) {
  const viewerState = useTranscriptViewer({
    alignment,
    hideAudioTags,
    segmentComposer,
    onPlay,
    onPause,
    onTimeUpdate,
    onEnded,
    onDurationChange,
  })

  const { audioRef } = viewerState

  const audioProps = useMemo(
    () => ({
      ref: audioRef,
      controls: false,
      preload: "metadata" as const,
      src: audioSrc,
      children: <source src={audioSrc} type={audioType} />,
    }),
    [audioRef, audioSrc]
  )

  const contextValue = useMemo(
    () => ({
      ...viewerState,
      audioProps,
    }),
    [viewerState, audioProps]
  )

  return (
    <TranscriptViewerProvider value={contextValue}>
      <div
        data-slot="transcript-viewer-root"
        className={cn("space-y-4 p-4", className)}
        {...props}
      >
        {children}
      </div>
    </TranscriptViewerProvider>
  )
}

type TranscriptViewerWordStatus = "spoken" | "unspoken" | "current"
interface TranscriptViewerWordProps
  extends Omit<HTMLAttributes<HTMLSpanElement>, "children"> {
  word: TranscriptWordType
  status: TranscriptViewerWordStatus
  children?: ReactNode
}

function TranscriptViewerWord({
  word,
  status,
  className,
  children,
  ...props
}: TranscriptViewerWordProps) {
  return (
    <span
      data-slot="transcript-word"
      data-kind="word"
      data-status={status}
      className={cn(
        "rounded-sm px-0.5 transition-colors",
        status === "spoken" && "text-foreground",
        status === "unspoken" && "text-muted-foreground",
        status === "current" && "bg-primary text-primary-foreground",
        className
      )}
      {...props}
    >
      {children ?? word.text}
    </span>
  )
}

interface TranscriptViewerWordsProps extends HTMLAttributes<HTMLDivElement> {
  renderWord?: (props: {
    word: TranscriptWordType
    status: TranscriptViewerWordStatus
  }) => ReactNode
  renderGap?: (props: {
    segment: TranscriptGap
    status: TranscriptViewerWordStatus
  }) => ReactNode
  wordClassNames?: string
  gapClassNames?: string
}

function TranscriptViewerWords({
  className,
  renderWord,
  renderGap,
  wordClassNames,
  gapClassNames,
  ...props
}: TranscriptViewerWordsProps) {
  const {
    spokenSegments,
    unspokenSegments,
    currentWord,
    segments,
    duration,
    currentTime,
  } = useTranscriptViewerContext()

  const nearEnd = useMemo(() => {
    if (!duration) return false
    return currentTime >= duration - 0.01
  }, [currentTime, duration])

  const segmentsWithStatus = useMemo(() => {
    if (nearEnd) {
      return segments.map((segment) => ({ segment, status: "spoken" as const }))
    }

    const entries: Array<{
      segment: TranscriptSegment
      status: TranscriptViewerWordStatus
    }> = []

    for (const segment of spokenSegments) {
      entries.push({ segment, status: "spoken" })
    }

    if (currentWord) {
      entries.push({ segment: currentWord, status: "current" })
    }

    for (const segment of unspokenSegments) {
      entries.push({ segment, status: "unspoken" })
    }

    return entries
  }, [spokenSegments, unspokenSegments, currentWord, nearEnd, segments])

  return (
    <div
      data-slot="transcript-words"
      className={cn("text-xl leading-relaxed", className)}
      {...props}
    >
      {segmentsWithStatus.map(({ segment, status }) => {
        if (segment.kind === "gap") {
          const content = renderGap
            ? renderGap({ segment, status })
            : segment.text
          return (
            <span
              key={`gap-${segment.segmentIndex}`}
              data-kind="gap"
              data-status={status}
              className={cn(gapClassNames)}
            >
              {content}
            </span>
          )
        }

        if (renderWord) {
          return (
            <span
              key={`word-${segment.segmentIndex}`}
              data-kind="word"
              data-status={status}
              className={cn(wordClassNames)}
            >
              {renderWord({ word: segment, status })}
            </span>
          )
        }

        return (
          <TranscriptViewerWord
            key={`word-${segment.segmentIndex}`}
            word={segment}
            status={status}
            className={wordClassNames}
          />
        )
      })}
    </div>
  )
}

function TranscriptViewerAudio({
  ...props
}: ComponentPropsWithoutRef<"audio">) {
  const { audioProps } = useTranscriptViewerContext()
  return (
    <audio
      data-slot="transcript-audio"
      {...audioProps}
      {...props}
      ref={audioProps.ref}
    />
  )
}

type RenderChildren = (state: { isPlaying: boolean }) => ReactNode

type TranscriptViewerPlayPauseButtonProps = Omit<
  ComponentPropsWithoutRef<typeof Button>,
  "children"
> & {
  children?: ReactNode | RenderChildren
}

function TranscriptViewerPlayPauseButton({
  className,
  children,
  onClick,
  ...props
}: TranscriptViewerPlayPauseButtonProps) {
  const { isPlaying, play, pause } = useTranscriptViewerContext()
  const Icon = isPlaying ? Pause : Play

  const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    if (isPlaying) pause()
    else play()
    onClick?.(event)
  }

  const content =
    typeof children === "function"
      ? (children as RenderChildren)({ isPlaying })
      : children

  return (
    <Button
      data-slot="transcript-play-pause-button"
      type="button"
      variant="outline"
      size="icon"
      aria-label={isPlaying ? "Pause audio" : "Play audio"}
      data-playing={isPlaying}
      className={cn("cursor-pointer", className)}
      onClick={handleClick}
      {...props}
    >
      {content ?? <Icon className="size-5" />}
    </Button>
  )
}

type TranscriptViewerScrubBarProps = Omit<
  ComponentPropsWithoutRef<typeof ScrubBarContainer>,
  "duration" | "value" | "onScrub" | "onScrubStart" | "onScrubEnd"
> & {
  showTimeLabels?: boolean
  labelsClassName?: string
  trackClassName?: string
  progressClassName?: string
  thumbClassName?: string
}

/**
 * A context-aware implementation of the scrub bar specific to the transcript viewer.
 */
function TranscriptViewerScrubBar({
  className,
  showTimeLabels = true,
  labelsClassName,
  trackClassName,
  progressClassName,
  thumbClassName,
  ...props
}: TranscriptViewerScrubBarProps) {
  const { duration, currentTime, seekToTime, startScrubbing, endScrubbing } =
    useTranscriptViewerContext()
  return (
    <ScrubBarContainer
      data-slot="transcript-scrub-bar"
      duration={duration}
      value={currentTime}
      onScrubStart={startScrubbing}
      onScrubEnd={endScrubbing}
      onScrub={seekToTime}
      className={className}
      {...props}
    >
      <div className="flex flex-1 flex-col gap-1">
        <ScrubBarTrack className={trackClassName}>
          <ScrubBarProgress className={progressClassName} />
          <ScrubBarThumb className={thumbClassName} />
        </ScrubBarTrack>
        {showTimeLabels && (
          <div
            className={cn(
              "text-muted-foreground flex items-center justify-between text-xs",
              labelsClassName
            )}
          >
            <ScrubBarTimeLabel time={currentTime} />
            <ScrubBarTimeLabel time={duration - currentTime} />
          </div>
        )}
      </div>
    </ScrubBarContainer>
  )
}

export {
  TranscriptViewerContainer,
  TranscriptViewerWords,
  TranscriptViewerWord,
  TranscriptViewerAudio,
  TranscriptViewerPlayPauseButton,
  TranscriptViewerScrubBar,
  TranscriptViewerProvider,
  useTranscriptViewerContext,
}
export type { CharacterAlignmentResponseModel }
```

---

### Voice Button <a name="voice-button"></a>

Interactive button with voice recording states, live waveform visualization, and automatic feedback transitions.

- **Registry Key**: `voice-button`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/voice-button.json
```

#### Dependencies

- **Local Registry Dependencies**:
  - `button`
  - `https://ui.elevenlabs.io/r/live-waveform.json`

#### How to Use

```tsx
type VoiceButtonState =
  | "idle"
  | "recording"
  | "processing"
  | "success"
  | "error"
```

#### Demo Code

##### File: `examples/voice-button-demo.tsx`

```tsx
"use client"

import { useEffect, useState } from "react"

import { VoiceButton } from "@/components/ui/voice-button"

export default function VoiceButtonDemo() {
  const [state, setState] = useState<
    "idle" | "recording" | "processing" | "success" | "error"
  >("idle")

  const handlePress = () => {
    if (state === "idle") {
      setState("recording")
    } else if (state === "recording") {
      setState("processing")

      setTimeout(() => {
        setState("success")

        setTimeout(() => {
          setState("idle")
        }, 1500)
      }, 1000)
    }
  }

  useEffect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.altKey && e.code === "Space") {
        e.preventDefault()
        handlePress()
      }
    }

    window.addEventListener("keydown", handleKeyDown)
    return () => window.removeEventListener("keydown", handleKeyDown)
  }, [state])

  return (
    <div className="flex min-h-[200px] w-full items-center justify-center">
      <VoiceButton
        label="Voice"
        trailing="⌥Space"
        state={state}
        onPress={handlePress}
        className="min-w-[180px]"
      />
    </div>
  )
}
```

#### Props & API Reference

##### `Props` Props

| Prop              | Type                                                                          | Default     | Description                                       |
| ----------------- | ----------------------------------------------------------------------------- | ----------- | ------------------------------------------------- |
| state             | `"idle" \| "recording" \| "processing" \| "success" \| "error"`               | `"idle"`    | Current state of the voice button                 |
| onPress           | `() => void`                                                                  | -           | Callback when button is clicked                   |
| label             | `ReactNode`                                                                   | -           | Content to display on the left side               |
| trailing          | `ReactNode`                                                                   | -           | Content to display on the right (e.g., shortcuts) |
| icon              | `ReactNode`                                                                   | -           | Icon to display when idle (for icon size buttons) |
| variant           | `"default" \| "destructive" \| "outline" \| "secondary" \| "ghost" \| "link"` | `"outline"` | Button variant                                    |
| size              | `"default" \| "sm" \| "lg" \| "icon"`                                         | `"default"` | Button size                                       |
| className         | `string`                                                                      | -           | Optional CSS classes for the button               |
| waveformClassName | `string`                                                                      | -           | Optional CSS classes for the waveform container   |
| feedbackDuration  | `number`                                                                      | `1500`      | Duration in ms to show success/error states       |
| ...props          | `HTMLButtonElement`                                                           | -           | All standard button element props                 |

#### Component Source Code

##### File: `components/ui/voice-button.tsx`

```tsx
"use client"

import * as React from "react"
import { CheckIcon, XIcon } from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { LiveWaveform } from "@/components/ui/live-waveform"

export type VoiceButtonState =
  | "idle"
  | "recording"
  | "processing"
  | "success"
  | "error"

export interface VoiceButtonProps
  extends Omit<React.ButtonHTMLAttributes<HTMLButtonElement>, "onError"> {
  /**
   * Current state of the voice button
   * @default "idle"
   */
  state?: VoiceButtonState

  /**
   * Callback when button is clicked
   */
  onPress?: () => void

  /**
   * Content to display on the left side (label)
   * Can be a string or ReactNode for custom components
   */
  label?: React.ReactNode

  /**
   * Content to display on the right side (e.g., keyboard shortcut)
   * Can be a string or ReactNode for custom components
   * @example "⌥Space" or <kbd>⌘K</kbd>
   */
  trailing?: React.ReactNode

  /**
   * Icon to display in the center when idle (for icon size buttons)
   */
  icon?: React.ReactNode

  /**
   * Custom variant for the button
   * @default "outline"
   */
  variant?:
    | "default"
    | "destructive"
    | "outline"
    | "secondary"
    | "ghost"
    | "link"

  /**
   * Size of the button
   * @default "default"
   */
  size?: "default" | "sm" | "lg" | "icon"

  /**
   * Custom className for the button
   */
  className?: string

  /**
   * Custom className for the waveform container
   */
  waveformClassName?: string

  /**
   * Duration in ms to show success/error states
   * @default 1500
   */
  feedbackDuration?: number

  /**
   * Disable the button
   */
  disabled?: boolean
}

export const VoiceButton = React.forwardRef<
  HTMLButtonElement,
  VoiceButtonProps
>(
  (
    {
      state = "idle",
      onPress,
      label,
      trailing,
      icon,
      variant = "outline",
      size = "default",
      className,
      waveformClassName,
      feedbackDuration = 1500,
      disabled,
      onClick,
      ...props
    },
    ref
  ) => {
    const [showFeedback, setShowFeedback] = React.useState(false)

    React.useEffect(() => {
      if (state === "success" || state === "error") {
        setShowFeedback(true)
        const timeout = setTimeout(
          () => setShowFeedback(false),
          feedbackDuration
        )
        return () => clearTimeout(timeout)
      } else {
        // Reset feedback when state changes away from success/error
        setShowFeedback(false)
      }
    }, [state, feedbackDuration])

    const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
      onClick?.(e)
      onPress?.()
    }

    const isRecording = state === "recording"
    const isProcessing = state === "processing"
    const isSuccess = state === "success"
    const isError = state === "error"

    const buttonVariant = variant
    const isDisabled = disabled || isProcessing

    const displayLabel = label

    const shouldShowWaveform = isRecording || isProcessing || showFeedback
    const shouldShowTrailing = !shouldShowWaveform && trailing

    return (
      <Button
        ref={ref}
        type="button"
        variant={buttonVariant}
        size={size}
        onClick={handleClick}
        disabled={isDisabled}
        className={cn(
          "gap-2 transition-all duration-200",
          size === "icon" && "relative",
          className
        )}
        aria-label={"Voice Button"}
        {...props}
      >
        {size !== "icon" && displayLabel && (
          <span className="inline-flex shrink-0 items-center justify-start">
            {displayLabel}
          </span>
        )}

        <div
          className={cn(
            "relative box-content flex shrink-0 items-center justify-center overflow-hidden transition-all duration-300",
            size === "icon"
              ? "absolute inset-0 rounded-sm border-0"
              : "h-5 w-24 rounded-sm border",
            isRecording
              ? "bg-primary/10 dark:bg-primary/5"
              : size === "icon"
                ? "bg-muted/50 border-0"
                : "border-border bg-muted/50",
            waveformClassName
          )}
        >
          {shouldShowWaveform && (
            <LiveWaveform
              active={isRecording}
              processing={isProcessing || isSuccess}
              barWidth={2}
              barGap={1}
              barRadius={4}
              fadeEdges={false}
              sensitivity={1.8}
              smoothingTimeConstant={0.85}
              height={20}
              mode="static"
              className="animate-in fade-in absolute inset-0 h-full w-full duration-300"
            />
          )}

          {shouldShowTrailing && (
            <div className="animate-in fade-in absolute inset-0 flex items-center justify-center duration-300">
              {typeof trailing === "string" ? (
                <span className="text-muted-foreground px-1.5 font-mono text-[10px] font-medium select-none">
                  {trailing}
                </span>
              ) : (
                trailing
              )}
            </div>
          )}

          {!shouldShowWaveform &&
            !shouldShowTrailing &&
            icon &&
            size === "icon" && (
              <div className="animate-in fade-in absolute inset-0 flex items-center justify-center duration-300">
                {icon}
              </div>
            )}

          {isSuccess && showFeedback && (
            <div className="animate-in fade-in bg-background/80 absolute inset-0 flex items-center justify-center duration-300">
              <span className="text-primary text-[10px] font-medium">
                <CheckIcon className="size-3.5" />
              </span>
            </div>
          )}

          {/* Error Icon */}
          {isError && showFeedback && (
            <div className="animate-in fade-in bg-background/80 absolute inset-0 flex items-center justify-center duration-300">
              <span className="text-destructive text-[10px] font-medium">
                <XIcon className="size-3.5" />
              </span>
            </div>
          )}
        </div>
      </Button>
    )
  }
)

VoiceButton.displayName = "VoiceButton"
```

---

### Voice Picker <a name="voice-picker"></a>

Searchable voice selector with audio preview, orb visualization, and ElevenLabs voice integration.

- **Registry Key**: `voice-picker`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/voice-picker.json
```

#### Dependencies

- **NPM Packages**:
  - `@elevenlabs/elevenlabs-js`
- **Local Registry Dependencies**:
  - `button`
  - `badge`
  - `command`
  - `popover`
  - `https://ui.elevenlabs.io/r/orb.json`
  - `https://ui.elevenlabs.io/r/audio-player.json`

#### Demo Code

##### File: `examples/voice-picker-demo.tsx`

```tsx
"use client"

import { useState } from "react"
import type { ElevenLabs } from "@elevenlabs/elevenlabs-js"

import { VoicePicker } from "@/components/ui/voice-picker"

const voices: ElevenLabs.Voice[] = [
  {
    voiceId: "21m00Tcm4TlvDq8ikWAM",
    name: "Rachel",
    category: "premade",
    labels: {
      accent: "american",
      descriptive: "casual",
      age: "young",
      gender: "female",
      language: "en",
      use_case: "conversational",
    },
    description:
      "Matter-of-fact, personable woman. Great for conversational use cases.",
    previewUrl:
      "https://storage.googleapis.com/eleven-public-prod/premade/voices/21m00Tcm4TlvDq8ikWAM/b4928a68-c03b-411f-8533-3d5c299fd451.mp3",
  },
  {
    voiceId: "29vD33N1CtxCmqQRPOHJ",
    name: "Drew",
    category: "premade",
    labels: {
      accent: "american",
      description: "well-rounded",
      age: "middle_aged",
      gender: "male",
      use_case: "news",
    },
    previewUrl:
      "https://storage.googleapis.com/eleven-public-prod/premade/voices/29vD33N1CtxCmqQRPOHJ/b99fc51d-12d3-4312-b480-a8a45a7d51ef.mp3",
  },
  {
    voiceId: "2EiwWnXFnvU5JabPnv8n",
    name: "Clyde",
    category: "premade",
    labels: {
      accent: "american",
      descriptive: "intense",
      age: "middle_aged",
      gender: "male",
      language: "en",
      use_case: "characters_animation",
    },
    description: "Great for character use-cases",
    previewUrl:
      "https://storage.googleapis.com/eleven-public-prod/premade/voices/2EiwWnXFnvU5JabPnv8n/65d80f52-703f-4cae-a91d-75d4e200ed02.mp3",
  },
]

export default function VoicePickerDemo() {
  const [selectedVoice, setSelectedVoice] = useState<string>(
    "21m00Tcm4TlvDq8ikWAM"
  )
  const [open, setOpen] = useState(false)

  return (
    <div className="w-full max-w-lg">
      <VoicePicker
        voices={voices}
        value={selectedVoice}
        onValueChange={(value) => {
          setSelectedVoice(value)
          // Keep dropdown open after selection
          setOpen(true)
        }}
        open={open}
        onOpenChange={setOpen}
        placeholder="Select a voice..."
      />
    </div>
  )
}
```

#### Props & API Reference

##### `Props` Props

| Prop          | Type                      | Default               | Description                                 |
| ------------- | ------------------------- | --------------------- | ------------------------------------------- |
| voices        | `ElevenLabs.Voice[]`      | -                     | **Required.** Array of ElevenLabs voices    |
| value         | `string`                  | -                     | Selected voice ID (controlled)              |
| onValueChange | `(value: string) => void` | -                     | Callback when selection changes             |
| placeholder   | `string`                  | `"Select a voice..."` | Placeholder text when no voice selected     |
| className     | `string`                  | -                     | Optional CSS classes for the trigger button |
| open          | `boolean`                 | -                     | Control popover open state                  |
| onOpenChange  | `(open: boolean) => void` | -                     | Callback when popover open state changes    |

#### Component Source Code

##### File: `components/ui/voice-picker.tsx`

```tsx
"use client"

import * as React from "react"
import type { ElevenLabs } from "@elevenlabs/elevenlabs-js"
import { Check, ChevronsUpDown, Pause, Play } from "lucide-react"

import { cn } from "@/lib/utils"
import {
  AudioPlayerProvider,
  useAudioPlayer,
} from "@/components/ui/audio-player"
import { Button } from "@/components/ui/button"
import {
  Command,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList,
} from "@/components/ui/command"
import { Orb } from "@/components/ui/orb"
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover"

interface VoicePickerProps {
  voices: ElevenLabs.Voice[]
  value?: string
  onValueChange?: (value: string) => void
  placeholder?: string
  className?: string
  open?: boolean
  onOpenChange?: (open: boolean) => void
}

function VoicePicker({
  voices,
  value,
  onValueChange,
  placeholder = "Select a voice...",
  className,
  open,
  onOpenChange,
}: VoicePickerProps) {
  const [internalOpen, setInternalOpen] = React.useState(false)
  const isControlled = open !== undefined
  const isOpen = isControlled ? open : internalOpen
  const setIsOpen = isControlled ? onOpenChange : setInternalOpen

  const selectedVoice = voices.find((v) => v.voiceId === value)

  return (
    <AudioPlayerProvider>
      <Popover open={isOpen} onOpenChange={setIsOpen}>
        <PopoverTrigger asChild>
          <Button
            variant="outline"
            role="combobox"
            aria-expanded={isOpen}
            className={cn("w-full justify-between", className)}
          >
            {selectedVoice ? (
              <div className="flex items-center gap-2 overflow-hidden">
                <div className="relative size-6 shrink-0 overflow-visible">
                  <Orb agentState="thinking" className="absolute inset-0" />
                </div>
                <span className="truncate">{selectedVoice.name}</span>
              </div>
            ) : (
              placeholder
            )}
            <ChevronsUpDown className="ml-2 size-4 shrink-0 opacity-50" />
          </Button>
        </PopoverTrigger>
        <PopoverContent className="w-[var(--radix-popover-trigger-width)] p-0">
          <Command>
            <CommandInput placeholder="Search voices..." />
            <CommandList>
              <CommandEmpty>No voice found.</CommandEmpty>
              <CommandGroup>
                {voices.map((voice) => (
                  <VoicePickerItem
                    key={voice.voiceId}
                    voice={voice}
                    isSelected={value === voice.voiceId}
                    onSelect={() => {
                      onValueChange?.(voice.voiceId!)
                    }}
                  />
                ))}
              </CommandGroup>
            </CommandList>
          </Command>
        </PopoverContent>
      </Popover>
    </AudioPlayerProvider>
  )
}

interface VoicePickerItemProps {
  voice: ElevenLabs.Voice
  isSelected: boolean
  onSelect: () => void
}

function VoicePickerItem({
  voice,
  isSelected,
  onSelect,
}: VoicePickerItemProps) {
  const [isHovered, setIsHovered] = React.useState(false)
  const player = useAudioPlayer()

  const preview = voice.previewUrl
  const audioItem = React.useMemo(
    () => (preview ? { id: voice.voiceId!, src: preview, data: voice } : null),
    [preview, voice]
  )

  const isPlaying =
    audioItem && player.isItemActive(audioItem.id) && player.isPlaying

  const handlePreview = React.useCallback(
    async (e: React.MouseEvent) => {
      e.preventDefault()
      e.stopPropagation()

      if (!audioItem) return

      if (isPlaying) {
        player.pause()
      } else {
        player.play(audioItem)
      }
    },
    [audioItem, isPlaying, player]
  )

  return (
    <CommandItem
      value={voice.voiceId!}
      keywords={[
        voice.name,
        voice.labels?.accent,
        voice.labels?.gender,
        voice.labels?.age,
        voice.labels?.description,
        voice.labels?.["use case"],
      ].filter((k): k is string => Boolean(k))}
      onSelect={onSelect}
      className="flex items-center gap-3"
    >
      <div
        className="relative z-10 size-8 shrink-0 cursor-pointer overflow-visible"
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
        onClick={handlePreview}
      >
        <Orb
          agentState={isPlaying ? "talking" : undefined}
          className="pointer-events-none absolute inset-0"
        />
        {preview && isHovered && (
          <div className="pointer-events-none absolute inset-0 flex size-8 shrink-0 items-center justify-center rounded-full bg-black/40 backdrop-blur-sm transition-opacity hover:bg-black/50">
            {isPlaying ? (
              <Pause className="size-3 text-white" />
            ) : (
              <Play className="size-3 text-white" />
            )}
          </div>
        )}
      </div>

      <div className="flex flex-1 flex-col gap-0.5">
        <span className="font-medium">{voice.name}</span>
        {voice.labels && (
          <div className="text-muted-foreground flex items-center gap-1.5 text-xs">
            {voice.labels.accent && <span>{voice.labels.accent}</span>}
            {voice.labels.gender && <span>•</span>}
            {voice.labels.gender && (
              <span className="capitalize">{voice.labels.gender}</span>
            )}
            {voice.labels.age && <span>•</span>}
            {voice.labels.age && (
              <span className="capitalize">{voice.labels.age}</span>
            )}
          </div>
        )}
      </div>

      <Check
        className={cn(
          "ml-auto size-4 shrink-0",
          isSelected ? "opacity-100" : "opacity-0"
        )}
      />
    </CommandItem>
  )
}

export { VoicePicker, VoicePickerItem }
```

---

### Waveform <a name="waveform"></a>

Canvas-based audio waveform visualization components with recording, playback scrubbing, and microphone input support.

- **Registry Key**: `waveform`

#### Installation

```bash
npx shadcn@latest add https://ui.elevenlabs.io/r/waveform.json
```

#### How to Use

##### Example 1

```tsx
<Waveform data={audioData} />
```

##### Example 2

```tsx
<ScrollingWaveform speed={50} />
```

##### Example 3

```tsx
<AudioScrubber
  data={waveformData}
  currentTime={playbackTime}
  duration={totalDuration}
  onSeek={handleSeek}
/>
```

##### Example 4

```tsx
<MicrophoneWaveform active={isListening} sensitivity={1.5} />
```

##### Example 5

```tsx
<StaticWaveform bars={40} seed={42} />
```

##### Example 6

```tsx
<LiveMicrophoneWaveform
  active={isRecording}
  enableAudioPlayback={true}
  playbackRate={1}
/>
```

##### Example 7

```tsx
<RecordingWaveform
  recording={isRecording}
  onRecordingComplete={(data) => console.log("Recording data:", data)}
/>
```

#### Demo Code

##### File: `examples/waveform-demo.tsx`

```tsx
"use client"

import { ScrollingWaveform } from "@/components/ui/waveform"

export default function WaveformDemo() {
  return (
    <div className="bg-card w-full rounded-lg border p-6">
      <div className="mb-4">
        <h3 className="text-lg font-semibold">Waveform</h3>
        <p className="text-muted-foreground text-sm">
          Real-time audio visualization with smooth scrolling animation
        </p>
      </div>
      <ScrollingWaveform
        height={80}
        barWidth={3}
        barGap={2}
        speed={30}
        fadeEdges={true}
        barColor="gray"
      />
    </div>
  )
}
```

#### Component Source Code

##### File: `components/ui/waveform.tsx`

```tsx
"use client"

import {
  useCallback,
  useEffect,
  useMemo,
  useRef,
  useState,
  type HTMLAttributes,
} from "react"

import { cn } from "@/lib/utils"

export type WaveformProps = HTMLAttributes<HTMLDivElement> & {
  data?: number[]
  barWidth?: number
  barHeight?: number
  barGap?: number
  barRadius?: number
  barColor?: string
  fadeEdges?: boolean
  fadeWidth?: number
  height?: string | number
  active?: boolean
  onBarClick?: (index: number, value: number) => void
}

export const Waveform = ({
  data = [],
  barWidth = 4,
  barHeight: baseBarHeight = 4,
  barGap = 2,
  barRadius = 2,
  barColor,
  fadeEdges = true,
  fadeWidth = 24,
  height = 128,
  onBarClick,
  className,
  ...props
}: WaveformProps) => {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const containerRef = useRef<HTMLDivElement>(null)
  const heightStyle = typeof height === "number" ? `${height}px` : height

  useEffect(() => {
    const canvas = canvasRef.current
    const container = containerRef.current
    if (!canvas || !container) return

    const resizeObserver = new ResizeObserver(() => {
      const rect = container.getBoundingClientRect()
      const dpr = window.devicePixelRatio || 1

      canvas.width = rect.width * dpr
      canvas.height = rect.height * dpr
      canvas.style.width = `${rect.width}px`
      canvas.style.height = `${rect.height}px`

      const ctx = canvas.getContext("2d")
      if (ctx) {
        ctx.scale(dpr, dpr)
        renderWaveform()
      }
    })

    const renderWaveform = () => {
      const ctx = canvas.getContext("2d")
      if (!ctx) return

      const rect = canvas.getBoundingClientRect()
      ctx.clearRect(0, 0, rect.width, rect.height)

      const computedBarColor =
        barColor ||
        getComputedStyle(canvas).getPropertyValue("--foreground") ||
        "#000"

      const barCount = Math.floor(rect.width / (barWidth + barGap))
      const centerY = rect.height / 2

      for (let i = 0; i < barCount; i++) {
        const dataIndex = Math.floor((i / barCount) * data.length)
        const value = data[dataIndex] || 0
        const barHeight = Math.max(baseBarHeight, value * rect.height * 0.8)
        const x = i * (barWidth + barGap)
        const y = centerY - barHeight / 2

        ctx.fillStyle = computedBarColor
        ctx.globalAlpha = 0.3 + value * 0.7

        if (barRadius > 0) {
          ctx.beginPath()
          ctx.roundRect(x, y, barWidth, barHeight, barRadius)
          ctx.fill()
        } else {
          ctx.fillRect(x, y, barWidth, barHeight)
        }
      }

      if (fadeEdges && fadeWidth > 0 && rect.width > 0) {
        const gradient = ctx.createLinearGradient(0, 0, rect.width, 0)
        const fadePercent = Math.min(0.2, fadeWidth / rect.width)

        gradient.addColorStop(0, "rgba(255,255,255,1)")
        gradient.addColorStop(fadePercent, "rgba(255,255,255,0)")
        gradient.addColorStop(1 - fadePercent, "rgba(255,255,255,0)")
        gradient.addColorStop(1, "rgba(255,255,255,1)")

        ctx.globalCompositeOperation = "destination-out"
        ctx.fillStyle = gradient
        ctx.fillRect(0, 0, rect.width, rect.height)
        ctx.globalCompositeOperation = "source-over"
      }

      ctx.globalAlpha = 1
    }

    resizeObserver.observe(container)
    renderWaveform()

    return () => resizeObserver.disconnect()
  }, [
    data,
    barWidth,
    baseBarHeight,
    barGap,
    barRadius,
    barColor,
    fadeEdges,
    fadeWidth,
  ])

  const handleClick = (e: React.MouseEvent<HTMLCanvasElement>) => {
    if (!onBarClick) return

    const rect = canvasRef.current?.getBoundingClientRect()
    if (!rect) return

    const x = e.clientX - rect.left
    const barIndex = Math.floor(x / (barWidth + barGap))
    const dataIndex = Math.floor(
      (barIndex * data.length) / Math.floor(rect.width / (barWidth + barGap))
    )

    if (dataIndex >= 0 && dataIndex < data.length) {
      onBarClick(dataIndex, data[dataIndex])
    }
  }

  return (
    <div
      className={cn("relative", className)}
      ref={containerRef}
      style={{ height: heightStyle }}
      {...props}
    >
      <canvas
        className="block h-full w-full"
        onClick={handleClick}
        ref={canvasRef}
      />
    </div>
  )
}

export type ScrollingWaveformProps = Omit<
  WaveformProps,
  "data" | "onBarClick"
> & {
  speed?: number
  barCount?: number
  data?: number[]
}

export const ScrollingWaveform = ({
  speed = 50,
  barCount = 60,
  barWidth = 4,
  barHeight: baseBarHeight = 4,
  barGap = 2,
  barRadius = 2,
  barColor,
  fadeEdges = true,
  fadeWidth = 24,
  height = 128,
  data,
  className,
  ...props
}: ScrollingWaveformProps) => {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const containerRef = useRef<HTMLDivElement>(null)
  const barsRef = useRef<Array<{ x: number; height: number }>>([])
  const animationRef = useRef<number>(0)
  const lastTimeRef = useRef<number>(0)
  const seedRef = useRef(Math.random())
  const dataIndexRef = useRef(0)
  const heightStyle = typeof height === "number" ? `${height}px` : height

  useEffect(() => {
    const canvas = canvasRef.current
    const container = containerRef.current
    if (!canvas || !container) return

    const resizeObserver = new ResizeObserver(() => {
      const rect = container.getBoundingClientRect()
      const dpr = window.devicePixelRatio || 1

      canvas.width = rect.width * dpr
      canvas.height = rect.height * dpr
      canvas.style.width = `${rect.width}px`
      canvas.style.height = `${rect.height}px`

      const ctx = canvas.getContext("2d")
      if (ctx) {
        ctx.scale(dpr, dpr)
      }

      if (barsRef.current.length === 0) {
        const step = barWidth + barGap
        let currentX = rect.width
        let index = 0
        const seededRandom = (i: number) => {
          const x = Math.sin(seedRef.current * 10000 + i) * 10000
          return x - Math.floor(x)
        }
        while (currentX > -step) {
          barsRef.current.push({
            x: currentX,
            height: 0.2 + seededRandom(index++) * 0.6,
          })
          currentX -= step
        }
      }
    })

    resizeObserver.observe(container)
    return () => resizeObserver.disconnect()
  }, [barWidth, barGap])

  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return

    const ctx = canvas.getContext("2d")
    if (!ctx) return

    const animate = (currentTime: number) => {
      const deltaTime = lastTimeRef.current
        ? (currentTime - lastTimeRef.current) / 1000
        : 0
      lastTimeRef.current = currentTime

      const rect = canvas.getBoundingClientRect()
      ctx.clearRect(0, 0, rect.width, rect.height)

      const computedBarColor =
        barColor ||
        getComputedStyle(canvas).getPropertyValue("--foreground") ||
        "#000"

      const step = barWidth + barGap
      for (let i = 0; i < barsRef.current.length; i++) {
        barsRef.current[i].x -= speed * deltaTime
      }

      barsRef.current = barsRef.current.filter(
        (bar) => bar.x + barWidth > -step
      )

      while (
        barsRef.current.length === 0 ||
        barsRef.current[barsRef.current.length - 1].x < rect.width
      ) {
        const lastBar = barsRef.current[barsRef.current.length - 1]
        const nextX = lastBar ? lastBar.x + step : rect.width

        let newHeight: number
        if (data && data.length > 0) {
          newHeight = data[dataIndexRef.current % data.length] || 0.1
          dataIndexRef.current = (dataIndexRef.current + 1) % data.length
        } else {
          const time = Date.now() / 1000
          const uniqueIndex = barsRef.current.length + time * 0.01
          const seededRandom = (index: number) => {
            const x = Math.sin(seedRef.current * 10000 + index * 137.5) * 10000
            return x - Math.floor(x)
          }
          const wave1 = Math.sin(uniqueIndex * 0.1) * 0.2
          const wave2 = Math.cos(uniqueIndex * 0.05) * 0.15
          const randomComponent = seededRandom(uniqueIndex) * 0.4
          newHeight = Math.max(
            0.1,
            Math.min(0.9, 0.3 + wave1 + wave2 + randomComponent)
          )
        }

        barsRef.current.push({
          x: nextX,
          height: newHeight,
        })
        if (barsRef.current.length > barCount * 2) break
      }

      const centerY = rect.height / 2
      for (const bar of barsRef.current) {
        if (bar.x < rect.width && bar.x + barWidth > 0) {
          const barHeight = Math.max(
            baseBarHeight,
            bar.height * rect.height * 0.6
          )
          const y = centerY - barHeight / 2

          ctx.fillStyle = computedBarColor
          ctx.globalAlpha = 0.3 + bar.height * 0.7

          if (barRadius > 0) {
            ctx.beginPath()
            ctx.roundRect(bar.x, y, barWidth, barHeight, barRadius)
            ctx.fill()
          } else {
            ctx.fillRect(bar.x, y, barWidth, barHeight)
          }
        }
      }

      if (fadeEdges && fadeWidth > 0) {
        const gradient = ctx.createLinearGradient(0, 0, rect.width, 0)
        const fadePercent = Math.min(0.2, fadeWidth / rect.width)

        gradient.addColorStop(0, "rgba(255,255,255,1)")
        gradient.addColorStop(fadePercent, "rgba(255,255,255,0)")
        gradient.addColorStop(1 - fadePercent, "rgba(255,255,255,0)")
        gradient.addColorStop(1, "rgba(255,255,255,1)")

        ctx.globalCompositeOperation = "destination-out"
        ctx.fillStyle = gradient
        ctx.fillRect(0, 0, rect.width, rect.height)
        ctx.globalCompositeOperation = "source-over"
      }

      ctx.globalAlpha = 1

      animationRef.current = requestAnimationFrame(animate)
    }

    animationRef.current = requestAnimationFrame(animate)

    return () => {
      if (animationRef.current) {
        cancelAnimationFrame(animationRef.current)
      }
    }
  }, [
    speed,
    barCount,
    barWidth,
    baseBarHeight,
    barGap,
    barRadius,
    barColor,
    fadeEdges,
    fadeWidth,
    data,
  ])

  return (
    <div
      className={cn("relative flex items-center", className)}
      ref={containerRef}
      style={{ height: heightStyle }}
      {...props}
    >
      <canvas className="block h-full w-full" ref={canvasRef} />
    </div>
  )
}

export type AudioScrubberProps = WaveformProps & {
  currentTime?: number
  duration?: number
  onSeek?: (time: number) => void
  showHandle?: boolean
}

export const AudioScrubber = ({
  data = [],
  currentTime = 0,
  duration = 100,
  onSeek,
  showHandle = true,
  barWidth = 3,
  barHeight,
  barGap = 1,
  barRadius = 1,
  barColor,
  height = 128,
  className,
  ...props
}: AudioScrubberProps) => {
  const [isDragging, setIsDragging] = useState(false)
  const [localProgress, setLocalProgress] = useState(0)
  const containerRef = useRef<HTMLDivElement>(null)

  const waveformData =
    data.length > 0
      ? data
      : Array.from({ length: 100 }, () => 0.2 + Math.random() * 0.6)

  useEffect(() => {
    if (!isDragging && duration > 0) {
      setLocalProgress(currentTime / duration)
    }
  }, [currentTime, duration, isDragging])

  const handleScrub = useCallback(
    (clientX: number) => {
      const container = containerRef.current
      if (!container) return

      const rect = container.getBoundingClientRect()
      const x = Math.max(0, Math.min(clientX - rect.left, rect.width))
      const progress = x / rect.width
      const newTime = progress * duration

      setLocalProgress(progress)
      onSeek?.(newTime)
    },
    [duration, onSeek]
  )

  const handleMouseDown = (e: React.MouseEvent<HTMLDivElement>) => {
    e.preventDefault()
    setIsDragging(true)
    handleScrub(e.clientX)
  }

  useEffect(() => {
    if (!isDragging) return

    const handleMouseMove = (e: MouseEvent) => {
      handleScrub(e.clientX)
    }

    const handleMouseUp = () => {
      setIsDragging(false)
    }

    document.addEventListener("mousemove", handleMouseMove)
    document.addEventListener("mouseup", handleMouseUp)

    return () => {
      document.removeEventListener("mousemove", handleMouseMove)
      document.removeEventListener("mouseup", handleMouseUp)
    }
  }, [isDragging, duration, handleScrub])

  const heightStyle = typeof height === "number" ? `${height}px` : height

  return (
    <div
      aria-label="Audio waveform scrubber"
      aria-valuemax={duration}
      aria-valuemin={0}
      aria-valuenow={currentTime}
      className={cn("relative cursor-pointer select-none", className)}
      onMouseDown={handleMouseDown}
      ref={containerRef}
      role="slider"
      style={{ height: heightStyle }}
      tabIndex={0}
      {...props}
    >
      <Waveform
        barColor={barColor}
        barGap={barGap}
        barRadius={barRadius}
        barWidth={barWidth}
        barHeight={barHeight}
        data={waveformData}
        fadeEdges={false}
      />

      <div
        className="bg-primary/20 pointer-events-none absolute inset-y-0 left-0"
        style={{ width: `${localProgress * 100}%` }}
      />

      <div
        className="bg-primary pointer-events-none absolute top-0 bottom-0 w-0.5"
        style={{ left: `${localProgress * 100}%` }}
      />

      {showHandle && (
        <div
          className="border-background bg-primary pointer-events-none absolute top-1/2 h-4 w-4 -translate-x-1/2 -translate-y-1/2 rounded-full border-2 shadow-lg transition-transform hover:scale-110"
          style={{ left: `${localProgress * 100}%` }}
        />
      )}
    </div>
  )
}

export type MicrophoneWaveformProps = WaveformProps & {
  active?: boolean
  processing?: boolean
  fftSize?: number
  smoothingTimeConstant?: number
  sensitivity?: number
  onError?: (error: Error) => void
}

export const MicrophoneWaveform = ({
  active = false,
  processing = false,
  fftSize = 256,
  smoothingTimeConstant = 0.8,
  sensitivity = 1,
  onError,
  ...props
}: MicrophoneWaveformProps) => {
  const [data, setData] = useState<number[]>([])
  const analyserRef = useRef<AnalyserNode | null>(null)
  const audioContextRef = useRef<AudioContext | null>(null)
  const streamRef = useRef<MediaStream | null>(null)
  const animationIdRef = useRef<number | null>(null)
  const processingAnimationRef = useRef<number | null>(null)
  const lastActiveDataRef = useRef<number[]>([])
  const transitionProgressRef = useRef(0)

  useEffect(() => {
    if (processing && !active) {
      let time = 0
      transitionProgressRef.current = 0

      const animateProcessing = () => {
        time += 0.03
        transitionProgressRef.current = Math.min(
          1,
          transitionProgressRef.current + 0.02
        )

        const processingData = []
        const barCount = 45

        for (let i = 0; i < barCount; i++) {
          const normalizedPosition = (i - barCount / 2) / (barCount / 2)
          const centerWeight = 1 - Math.abs(normalizedPosition) * 0.4

          const wave1 = Math.sin(time * 1.5 + i * 0.15) * 0.25
          const wave2 = Math.sin(time * 0.8 - i * 0.1) * 0.2
          const wave3 = Math.cos(time * 2 + i * 0.05) * 0.15
          const combinedWave = wave1 + wave2 + wave3
          const processingValue = (0.2 + combinedWave) * centerWeight

          let finalValue = processingValue
          if (
            lastActiveDataRef.current.length > 0 &&
            transitionProgressRef.current < 1
          ) {
            const lastDataIndex = Math.floor(
              (i / barCount) * lastActiveDataRef.current.length
            )
            const lastValue = lastActiveDataRef.current[lastDataIndex] || 0
            finalValue =
              lastValue * (1 - transitionProgressRef.current) +
              processingValue * transitionProgressRef.current
          }

          processingData.push(Math.max(0.05, Math.min(1, finalValue)))
        }

        setData(processingData)
        processingAnimationRef.current =
          requestAnimationFrame(animateProcessing)
      }

      animateProcessing()

      return () => {
        if (processingAnimationRef.current) {
          cancelAnimationFrame(processingAnimationRef.current)
        }
      }
    } else if (!active && !processing) {
      if (data.length > 0) {
        let fadeProgress = 0
        const fadeToIdle = () => {
          fadeProgress += 0.03
          if (fadeProgress < 1) {
            const fadedData = data.map((value) => value * (1 - fadeProgress))
            setData(fadedData)
            requestAnimationFrame(fadeToIdle)
          } else {
            setData([])
          }
        }
        fadeToIdle()
      }
      return
    }
  }, [processing, active])

  useEffect(() => {
    if (!active) {
      if (streamRef.current) {
        streamRef.current.getTracks().forEach((track) => track.stop())
      }
      if (
        audioContextRef.current &&
        audioContextRef.current.state !== "closed"
      ) {
        audioContextRef.current.close()
      }
      if (animationIdRef.current) {
        cancelAnimationFrame(animationIdRef.current)
      }
      return
    }

    const setupMicrophone = async () => {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({
          audio: true,
        })
        streamRef.current = stream

        const audioContext = new (window.AudioContext ||
          (window as unknown as { webkitAudioContext: typeof AudioContext })
            .webkitAudioContext)()
        const analyser = audioContext.createAnalyser()
        analyser.fftSize = fftSize
        analyser.smoothingTimeConstant = smoothingTimeConstant

        const source = audioContext.createMediaStreamSource(stream)
        source.connect(analyser)

        audioContextRef.current = audioContext
        analyserRef.current = analyser

        const dataArray = new Uint8Array(analyser.frequencyBinCount)

        const updateData = () => {
          if (!analyserRef.current || !active) return

          analyserRef.current.getByteFrequencyData(dataArray)

          const startFreq = Math.floor(dataArray.length * 0.05)
          const endFreq = Math.floor(dataArray.length * 0.4)
          const relevantData = dataArray.slice(startFreq, endFreq)

          const halfLength = Math.floor(relevantData.length / 2)
          const normalizedData = []

          for (let i = halfLength - 1; i >= 0; i--) {
            const value = Math.min(1, (relevantData[i] / 255) * sensitivity)
            normalizedData.push(value)
          }

          for (let i = 0; i < halfLength; i++) {
            const value = Math.min(1, (relevantData[i] / 255) * sensitivity)
            normalizedData.push(value)
          }

          setData(normalizedData)
          lastActiveDataRef.current = normalizedData

          animationIdRef.current = requestAnimationFrame(updateData)
        }

        updateData()
      } catch (error) {
        onError?.(error as Error)
      }
    }

    setupMicrophone()

    return () => {
      if (streamRef.current) {
        streamRef.current.getTracks().forEach((track) => track.stop())
      }
      if (
        audioContextRef.current &&
        audioContextRef.current.state !== "closed"
      ) {
        audioContextRef.current.close()
      }
      if (animationIdRef.current) {
        cancelAnimationFrame(animationIdRef.current)
      }
    }
  }, [active, fftSize, smoothingTimeConstant, sensitivity, onError])

  return <Waveform data={data} {...props} />
}

export type StaticWaveformProps = WaveformProps & {
  bars?: number
  seed?: number
}

export const StaticWaveform = ({
  bars = 40,
  seed = 42,
  ...props
}: StaticWaveformProps) => {
  const data = useMemo(() => {
    const random = (seedValue: number) => {
      const x = Math.sin(seedValue) * 10000
      return x - Math.floor(x)
    }

    return Array.from({ length: bars }, (_, i) => 0.2 + random(seed + i) * 0.6)
  }, [bars, seed])

  return <Waveform data={data} {...props} />
}

export type LiveMicrophoneWaveformProps = Omit<
  ScrollingWaveformProps,
  "barCount"
> & {
  active?: boolean
  fftSize?: number
  smoothingTimeConstant?: number
  sensitivity?: number
  onError?: (error: Error) => void
  historySize?: number
  updateRate?: number
  savedHistoryRef?: React.MutableRefObject<number[]>
  dragOffset?: number
  setDragOffset?: (offset: number) => void
  enableAudioPlayback?: boolean
  playbackRate?: number
}

export const LiveMicrophoneWaveform = ({
  active = false,
  fftSize = 256,
  smoothingTimeConstant = 0.8,
  sensitivity = 1,
  onError,
  historySize = 150,
  updateRate = 50,
  barWidth = 3,
  barHeight: baseBarHeight = 4,
  barGap = 1,
  barRadius = 1,
  barColor,
  fadeEdges = true,
  fadeWidth = 24,
  height = 128,
  className,
  savedHistoryRef,
  dragOffset: externalDragOffset,
  setDragOffset: externalSetDragOffset,
  enableAudioPlayback = true,
  playbackRate = 1,
  ...props
}: LiveMicrophoneWaveformProps) => {
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const containerRef = useRef<HTMLDivElement>(null)
  const internalHistoryRef = useRef<number[]>([])
  const historyRef = savedHistoryRef || internalHistoryRef
  const analyserRef = useRef<AnalyserNode | null>(null)
  const audioContextRef = useRef<AudioContext | null>(null)
  const streamRef = useRef<MediaStream | null>(null)
  const animationRef = useRef<number>(0)
  const lastUpdateRef = useRef<number>(0)
  const [internalDragOffset, setInternalDragOffset] = useState(0)
  const [isDragging, setIsDragging] = useState(false)
  const [playbackPosition, setPlaybackPosition] = useState<number | null>(null)
  const dragStartXRef = useRef<number>(0)
  const dragStartOffsetRef = useRef<number>(0)
  const playbackStartTimeRef = useRef<number>(0)

  // Audio recording and playback refs
  const mediaRecorderRef = useRef<MediaRecorder | null>(null)
  const audioChunksRef = useRef<Blob[]>([])
  const audioBufferRef = useRef<AudioBuffer | null>(null)
  const sourceNodeRef = useRef<AudioBufferSourceNode | null>(null)
  const scrubSourceRef = useRef<AudioBufferSourceNode | null>(null)

  // Use external drag state if provided, otherwise use internal
  const dragOffset = externalDragOffset ?? internalDragOffset
  const setDragOffset = externalSetDragOffset ?? setInternalDragOffset

  const heightStyle = typeof height === "number" ? `${height}px` : height

  useEffect(() => {
    const canvas = canvasRef.current
    const container = containerRef.current
    if (!canvas || !container) return

    const resizeObserver = new ResizeObserver(() => {
      const rect = container.getBoundingClientRect()
      const dpr = window.devicePixelRatio || 1

      canvas.width = rect.width * dpr
      canvas.height = rect.height * dpr
      canvas.style.width = `${rect.width}px`
      canvas.style.height = `${rect.height}px`

      const ctx = canvas.getContext("2d")
      if (ctx) {
        ctx.scale(dpr, dpr)
      }
    })

    resizeObserver.observe(container)
    return () => resizeObserver.disconnect()
  }, [])

  useEffect(() => {
    if (!active) {
      if (
        mediaRecorderRef.current &&
        mediaRecorderRef.current.state !== "inactive"
      ) {
        mediaRecorderRef.current.stop()
      }
      if (streamRef.current) {
        streamRef.current.getTracks().forEach((track) => track.stop())
      }
      // Process recorded audio when stopping
      if (enableAudioPlayback && audioChunksRef.current.length > 0) {
        const audioBlob = new Blob(audioChunksRef.current, {
          type: "audio/webm",
        })
        processAudioBlob(audioBlob)
      }
      return
    }

    setDragOffset?.(0)
    historyRef.current = []
    audioChunksRef.current = []
    audioBufferRef.current = null
    setPlaybackPosition(null)

    const setupMicrophone = async () => {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({
          audio: true,
        })
        streamRef.current = stream

        const audioContext = new (window.AudioContext ||
          (window as unknown as { webkitAudioContext: typeof AudioContext })
            .webkitAudioContext)()
        const analyser = audioContext.createAnalyser()
        analyser.fftSize = fftSize
        analyser.smoothingTimeConstant = smoothingTimeConstant

        const source = audioContext.createMediaStreamSource(stream)
        source.connect(analyser)

        audioContextRef.current = audioContext
        analyserRef.current = analyser

        if (enableAudioPlayback) {
          const mediaRecorder = new MediaRecorder(stream)
          mediaRecorderRef.current = mediaRecorder

          mediaRecorder.ondataavailable = (event) => {
            if (event.data.size > 0) {
              audioChunksRef.current.push(event.data)
            }
          }

          mediaRecorder.start(100)
        }
      } catch (error) {
        onError?.(error as Error)
      }
    }

    setupMicrophone()

    return () => {
      if (
        mediaRecorderRef.current &&
        mediaRecorderRef.current.state !== "inactive"
      ) {
        mediaRecorderRef.current.stop()
      }
      if (streamRef.current) {
        streamRef.current.getTracks().forEach((track) => track.stop())
      }
      if (sourceNodeRef.current) {
        sourceNodeRef.current.stop()
      }
      if (scrubSourceRef.current) {
        scrubSourceRef.current.stop()
      }
    }
  }, [
    active,
    fftSize,
    smoothingTimeConstant,
    onError,
    setDragOffset,
    enableAudioPlayback,
    historyRef,
  ])

  const processAudioBlob = async (blob: Blob) => {
    try {
      const arrayBuffer = await blob.arrayBuffer()
      if (audioContextRef.current) {
        const audioBuffer =
          await audioContextRef.current.decodeAudioData(arrayBuffer)
        audioBufferRef.current = audioBuffer
      }
    } catch (error) {
      console.error("Error processing audio:", error)
    }
  }

  const playScrubSound = useCallback(
    (position: number, direction: number) => {
      if (
        !enableAudioPlayback ||
        !audioBufferRef.current ||
        !audioContextRef.current
      )
        return

      if (scrubSourceRef.current) {
        try {
          scrubSourceRef.current.stop()
        } catch {}
      }

      const source = audioContextRef.current.createBufferSource()
      source.buffer = audioBufferRef.current

      const speed = Math.abs(direction)
      const playbackRate =
        direction > 0
          ? Math.min(3, 1 + speed * 0.1)
          : Math.max(-3, -1 - speed * 0.1)

      source.playbackRate.value = playbackRate

      const filter = audioContextRef.current.createBiquadFilter()
      filter.type = "lowpass"
      filter.frequency.value = Math.max(200, 2000 - speed * 100)

      source.connect(filter)
      filter.connect(audioContextRef.current.destination)

      const startTime = Math.max(
        0,
        Math.min(position, audioBufferRef.current.duration - 0.1)
      )
      source.start(0, startTime, 0.1)
      scrubSourceRef.current = source
    },
    [enableAudioPlayback]
  )

  const playFromPosition = useCallback(
    (position: number) => {
      if (
        !enableAudioPlayback ||
        !audioBufferRef.current ||
        !audioContextRef.current
      )
        return

      if (sourceNodeRef.current) {
        try {
          sourceNodeRef.current.stop()
        } catch {}
      }

      const source = audioContextRef.current.createBufferSource()
      source.buffer = audioBufferRef.current
      source.playbackRate.value = playbackRate
      source.connect(audioContextRef.current.destination)

      const startTime = Math.max(
        0,
        Math.min(position, audioBufferRef.current.duration)
      )
      source.start(0, startTime)
      sourceNodeRef.current = source

      playbackStartTimeRef.current =
        audioContextRef.current.currentTime - startTime
      setPlaybackPosition(startTime)

      source.onended = () => {
        setPlaybackPosition(null)
      }
    },
    [enableAudioPlayback, playbackRate]
  )

  useEffect(() => {
    if (playbackPosition === null || !audioBufferRef.current) return

    let animationId: number
    const updatePlaybackVisual = () => {
      if (
        audioContextRef.current &&
        sourceNodeRef.current &&
        audioBufferRef.current
      ) {
        const elapsed =
          audioContextRef.current.currentTime - playbackStartTimeRef.current
        const currentPos = playbackPosition + elapsed * playbackRate

        if (currentPos < audioBufferRef.current.duration) {
          const progressRatio = currentPos / audioBufferRef.current.duration
          const currentBarIndex = Math.floor(
            progressRatio * historyRef.current.length
          )
          const step = barWidth + barGap

          const containerWidth =
            containerRef.current?.getBoundingClientRect().width || 0
          const viewBars = Math.floor(containerWidth / step)
          const targetOffset =
            -(currentBarIndex - (historyRef.current.length - viewBars)) * step
          const clampedOffset = Math.max(
            -(historyRef.current.length - viewBars) * step,
            Math.min(0, targetOffset)
          )

          setDragOffset?.(clampedOffset)
          animationId = requestAnimationFrame(updatePlaybackVisual)
        } else {
          setPlaybackPosition(null)
          const step = barWidth + barGap
          const containerWidth =
            containerRef.current?.getBoundingClientRect().width || 0
          const viewBars = Math.floor(containerWidth / step)
          setDragOffset?.(-(historyRef.current.length - viewBars) * step)
        }
      }
    }

    animationId = requestAnimationFrame(updatePlaybackVisual)

    return () => {
      if (animationId) cancelAnimationFrame(animationId)
    }
  }, [
    playbackPosition,
    playbackRate,
    barWidth,
    baseBarHeight,
    barGap,
    setDragOffset,
    historyRef,
  ])

  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return
    if (!active && historyRef.current.length === 0 && playbackPosition === null)
      return

    const ctx = canvas.getContext("2d")
    if (!ctx) return

    const animate = (currentTime: number) => {
      if (active && currentTime - lastUpdateRef.current > updateRate) {
        lastUpdateRef.current = currentTime

        if (analyserRef.current) {
          const dataArray = new Uint8Array(
            analyserRef.current.frequencyBinCount
          )
          analyserRef.current.getByteFrequencyData(dataArray)

          let sum = 0
          for (let i = 0; i < dataArray.length; i++) {
            sum += dataArray[i]
          }
          const average = (sum / dataArray.length / 255) * sensitivity

          historyRef.current.push(Math.min(1, Math.max(0.05, average)))

          if (historyRef.current.length > historySize) {
            historyRef.current.shift()
          }
        }
      }

      const rect = canvas.getBoundingClientRect()
      ctx.clearRect(0, 0, rect.width, rect.height)

      const computedBarColor =
        barColor ||
        getComputedStyle(canvas).getPropertyValue("--foreground") ||
        "#000"

      const step = barWidth + barGap
      const barCount = Math.floor(rect.width / step)
      const centerY = rect.height / 2

      const dataToRender = historyRef.current

      if (dataToRender.length > 0) {
        const offsetInBars = Math.floor(dragOffset / step)

        for (let i = 0; i < barCount; i++) {
          let dataIndex

          if (active) {
            dataIndex = dataToRender.length - 1 - i
          } else {
            dataIndex = Math.max(
              0,
              Math.min(
                dataToRender.length - 1,
                dataToRender.length - 1 - i - Math.floor(offsetInBars)
              )
            )
          }

          if (dataIndex >= 0 && dataIndex < dataToRender.length) {
            const value = dataToRender[dataIndex]
            if (value !== undefined) {
              const x = rect.width - (i + 1) * step
              const barHeight = Math.max(
                baseBarHeight,
                value * rect.height * 0.7
              )
              const y = centerY - barHeight / 2

              ctx.fillStyle = computedBarColor
              ctx.globalAlpha = 0.3 + value * 0.7

              if (barRadius > 0) {
                ctx.beginPath()
                ctx.roundRect(x, y, barWidth, barHeight, barRadius)
                ctx.fill()
              } else {
                ctx.fillRect(x, y, barWidth, barHeight)
              }
            }
          }
        }
      }

      if (fadeEdges && fadeWidth > 0) {
        const gradient = ctx.createLinearGradient(0, 0, rect.width, 0)
        const fadePercent = Math.min(0.2, fadeWidth / rect.width)

        gradient.addColorStop(0, "rgba(255,255,255,1)")
        gradient.addColorStop(fadePercent, "rgba(255,255,255,0)")
        gradient.addColorStop(1 - fadePercent, "rgba(255,255,255,0)")
        gradient.addColorStop(1, "rgba(255,255,255,1)")

        ctx.globalCompositeOperation = "destination-out"
        ctx.fillStyle = gradient
        ctx.fillRect(0, 0, rect.width, rect.height)
        ctx.globalCompositeOperation = "source-over"
      }

      ctx.globalAlpha = 1

      animationRef.current = requestAnimationFrame(animate)
    }

    if (active || historyRef.current.length > 0 || playbackPosition !== null) {
      animationRef.current = requestAnimationFrame(animate)
    }

    return () => {
      if (animationRef.current) {
        cancelAnimationFrame(animationRef.current)
      }
    }
  }, [
    active,
    sensitivity,
    updateRate,
    historySize,
    barWidth,
    baseBarHeight,
    barGap,
    barRadius,
    barColor,
    fadeEdges,
    fadeWidth,
    dragOffset,
    playbackPosition,
    historyRef,
  ])

  const handleMouseDown = (e: React.MouseEvent<HTMLDivElement>) => {
    if (active || historyRef.current.length === 0) return

    e.preventDefault()
    setIsDragging(true)
    dragStartXRef.current = e.clientX
    dragStartOffsetRef.current = dragOffset
  }

  useEffect(() => {
    if (!isDragging) return

    let lastScrubTime = 0
    let lastMouseX = dragStartXRef.current
    const handleMouseMove = (e: MouseEvent) => {
      const deltaX = e.clientX - dragStartXRef.current
      const newOffset = dragStartOffsetRef.current - deltaX * 0.5 // Reduce sensitivity

      const step = barWidth + barGap
      const maxBars = historyRef.current.length
      const viewWidth = canvasRef.current?.getBoundingClientRect().width || 0
      const viewBars = Math.floor(viewWidth / step)

      const maxOffset = Math.max(0, (maxBars - viewBars) * step)
      const minOffset = 0
      const clampedOffset = Math.max(minOffset, Math.min(maxOffset, newOffset))

      setDragOffset?.(clampedOffset)

      const now = Date.now()
      if (
        enableAudioPlayback &&
        audioBufferRef.current &&
        now - lastScrubTime > 50
      ) {
        lastScrubTime = now
        const offsetBars = Math.floor(clampedOffset / step)
        const rightmostBarIndex = Math.max(
          0,
          Math.min(maxBars - 1, maxBars - 1 - offsetBars)
        )
        const audioPosition =
          (rightmostBarIndex / maxBars) * audioBufferRef.current.duration
        const direction = e.clientX - lastMouseX
        lastMouseX = e.clientX
        playScrubSound(
          Math.max(
            0,
            Math.min(audioBufferRef.current.duration - 0.1, audioPosition)
          ),
          direction
        )
      }
    }

    const handleMouseUp = () => {
      setIsDragging(false)

      if (enableAudioPlayback && audioBufferRef.current) {
        const step = barWidth + barGap
        const maxBars = historyRef.current.length
        const offsetBars = Math.floor(dragOffset / step)
        const rightmostBarIndex = Math.max(
          0,
          Math.min(maxBars - 1, maxBars - 1 - offsetBars)
        )
        const audioPosition =
          (rightmostBarIndex / maxBars) * audioBufferRef.current.duration
        playFromPosition(
          Math.max(
            0,
            Math.min(audioBufferRef.current.duration - 0.1, audioPosition)
          )
        )
      }

      if (scrubSourceRef.current) {
        try {
          scrubSourceRef.current.stop()
        } catch {}
      }
    }

    document.addEventListener("mousemove", handleMouseMove)
    document.addEventListener("mouseup", handleMouseUp)

    return () => {
      document.removeEventListener("mousemove", handleMouseMove)
      document.removeEventListener("mouseup", handleMouseUp)
    }
  }, [
    isDragging,
    barWidth,
    barGap,
    setDragOffset,
    dragOffset,
    enableAudioPlayback,
    playScrubSound,
    playFromPosition,
    historyRef,
  ])

  return (
    <div
      className={cn(
        "relative flex items-center",
        !active && historyRef.current.length > 0 && "cursor-pointer",
        className
      )}
      onMouseDown={handleMouseDown}
      ref={containerRef}
      role={!active && historyRef.current.length > 0 ? "slider" : undefined}
      aria-label={
        !active && historyRef.current.length > 0
          ? "Drag to scrub through recording"
          : undefined
      }
      aria-valuenow={
        !active && historyRef.current.length > 0
          ? Math.abs(dragOffset)
          : undefined
      }
      aria-valuemin={!active && historyRef.current.length > 0 ? 0 : undefined}
      aria-valuemax={
        !active && historyRef.current.length > 0
          ? historyRef.current.length
          : undefined
      }
      tabIndex={!active && historyRef.current.length > 0 ? 0 : undefined}
      style={{ height: heightStyle }}
      {...props}
    >
      <canvas className="block h-full w-full" ref={canvasRef} />
    </div>
  )
}

export type RecordingWaveformProps = Omit<
  WaveformProps,
  "data" | "onBarClick"
> & {
  recording?: boolean
  fftSize?: number
  smoothingTimeConstant?: number
  sensitivity?: number
  onError?: (error: Error) => void
  onRecordingComplete?: (data: number[]) => void
  updateRate?: number
  showHandle?: boolean
}

export const RecordingWaveform = ({
  recording = false,
  fftSize = 256,
  smoothingTimeConstant = 0.8,
  sensitivity = 1,
  onError,
  onRecordingComplete,
  updateRate = 50,
  showHandle = true,
  barWidth = 3,
  barHeight: baseBarHeight = 4,
  barGap = 1,
  barRadius = 1,
  barColor,
  height = 128,
  className,
  ...props
}: RecordingWaveformProps) => {
  const [recordedData, setRecordedData] = useState<number[]>([])
  const [viewPosition, setViewPosition] = useState(1)
  const [isDragging, setIsDragging] = useState(false)
  const [isRecordingComplete, setIsRecordingComplete] = useState(false)

  const canvasRef = useRef<HTMLCanvasElement>(null)
  const containerRef = useRef<HTMLDivElement>(null)
  const recordingDataRef = useRef<number[]>([])
  const analyserRef = useRef<AnalyserNode | null>(null)
  const audioContextRef = useRef<AudioContext | null>(null)
  const streamRef = useRef<MediaStream | null>(null)
  const animationRef = useRef<number>(0)
  const lastUpdateRef = useRef<number>(0)

  const heightStyle = typeof height === "number" ? `${height}px` : height

  useEffect(() => {
    const canvas = canvasRef.current
    const container = containerRef.current
    if (!canvas || !container) return

    const resizeObserver = new ResizeObserver(() => {
      const rect = container.getBoundingClientRect()
      const dpr = window.devicePixelRatio || 1

      canvas.width = rect.width * dpr
      canvas.height = rect.height * dpr
      canvas.style.width = `${rect.width}px`
      canvas.style.height = `${rect.height}px`

      const ctx = canvas.getContext("2d")
      if (ctx) {
        ctx.scale(dpr, dpr)
      }
    })

    resizeObserver.observe(container)
    return () => resizeObserver.disconnect()
  }, [])

  useEffect(() => {
    if (!recording) {
      if (streamRef.current) {
        streamRef.current.getTracks().forEach((track) => track.stop())
      }
      if (
        audioContextRef.current &&
        audioContextRef.current.state !== "closed"
      ) {
        audioContextRef.current.close()
      }

      if (recordingDataRef.current.length > 0) {
        setRecordedData([...recordingDataRef.current])
        setIsRecordingComplete(true)
        onRecordingComplete?.(recordingDataRef.current)
      }
      return
    }

    setIsRecordingComplete(false)
    recordingDataRef.current = []
    setRecordedData([])
    setViewPosition(1)

    const setupMicrophone = async () => {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({
          audio: true,
        })
        streamRef.current = stream

        const audioContext = new (window.AudioContext ||
          (window as unknown as { webkitAudioContext: typeof AudioContext })
            .webkitAudioContext)()
        const analyser = audioContext.createAnalyser()
        analyser.fftSize = fftSize
        analyser.smoothingTimeConstant = smoothingTimeConstant

        const source = audioContext.createMediaStreamSource(stream)
        source.connect(analyser)

        audioContextRef.current = audioContext
        analyserRef.current = analyser
      } catch (error) {
        onError?.(error as Error)
      }
    }

    setupMicrophone()

    return () => {
      if (streamRef.current) {
        streamRef.current.getTracks().forEach((track) => track.stop())
      }
      if (
        audioContextRef.current &&
        audioContextRef.current.state !== "closed"
      ) {
        audioContextRef.current.close()
      }
    }
  }, [recording, fftSize, smoothingTimeConstant, onError, onRecordingComplete])

  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return

    const ctx = canvas.getContext("2d")
    if (!ctx) return

    const animate = (currentTime: number) => {
      if (recording && currentTime - lastUpdateRef.current > updateRate) {
        lastUpdateRef.current = currentTime

        if (analyserRef.current) {
          const dataArray = new Uint8Array(
            analyserRef.current.frequencyBinCount
          )
          analyserRef.current.getByteFrequencyData(dataArray)

          let sum = 0
          for (let i = 0; i < dataArray.length; i++) {
            sum += dataArray[i]
          }
          const average = (sum / dataArray.length / 255) * sensitivity

          recordingDataRef.current.push(Math.min(1, Math.max(0.05, average)))
        }
      }

      const rect = canvas.getBoundingClientRect()
      ctx.clearRect(0, 0, rect.width, rect.height)

      const computedBarColor =
        barColor ||
        getComputedStyle(canvas).getPropertyValue("--foreground") ||
        "#000"

      const dataToRender = recording ? recordingDataRef.current : recordedData

      if (dataToRender.length > 0) {
        const step = barWidth + barGap
        const barsVisible = Math.floor(rect.width / step)
        const centerY = rect.height / 2

        let startIndex = 0
        if (!recording && isRecordingComplete) {
          const totalBars = dataToRender.length
          if (totalBars > barsVisible) {
            startIndex = Math.floor((totalBars - barsVisible) * viewPosition)
          }
        } else if (recording) {
          startIndex = Math.max(0, dataToRender.length - barsVisible)
        }

        for (
          let i = 0;
          i < barsVisible && startIndex + i < dataToRender.length;
          i++
        ) {
          const value = dataToRender[startIndex + i] || 0.1
          const x = i * step
          const barHeight = Math.max(baseBarHeight, value * rect.height * 0.7)
          const y = centerY - barHeight / 2

          ctx.fillStyle = computedBarColor
          ctx.globalAlpha = 0.3 + value * 0.7

          if (barRadius > 0) {
            ctx.beginPath()
            ctx.roundRect(x, y, barWidth, barHeight, barRadius)
            ctx.fill()
          } else {
            ctx.fillRect(x, y, barWidth, barHeight)
          }
        }

        if (!recording && isRecordingComplete && showHandle) {
          const indicatorX = rect.width * viewPosition

          ctx.strokeStyle = computedBarColor
          ctx.globalAlpha = 0.5
          ctx.lineWidth = 2
          ctx.beginPath()
          ctx.moveTo(indicatorX, 0)
          ctx.lineTo(indicatorX, rect.height)
          ctx.stroke()
          ctx.fillStyle = computedBarColor
          ctx.globalAlpha = 1
          ctx.beginPath()
          ctx.arc(indicatorX, centerY, 6, 0, Math.PI * 2)
          ctx.fill()
        }
      }

      ctx.globalAlpha = 1

      animationRef.current = requestAnimationFrame(animate)
    }

    animationRef.current = requestAnimationFrame(animate)

    return () => {
      if (animationRef.current) {
        cancelAnimationFrame(animationRef.current)
      }
    }
  }, [
    recording,
    recordedData,
    viewPosition,
    isRecordingComplete,
    sensitivity,
    updateRate,
    showHandle,
    barWidth,
    baseBarHeight,
    barGap,
    barRadius,
    barColor,
  ])

  const handleScrub = useCallback(
    (clientX: number) => {
      const container = containerRef.current
      if (!container || recording || !isRecordingComplete) return

      const rect = container.getBoundingClientRect()
      const x = Math.max(0, Math.min(clientX - rect.left, rect.width))
      const position = x / rect.width

      setViewPosition(position)
    },
    [recording, isRecordingComplete]
  )

  const handleMouseDown = (e: React.MouseEvent<HTMLDivElement>) => {
    if (recording || !isRecordingComplete) return

    e.preventDefault()
    setIsDragging(true)
    handleScrub(e.clientX)
  }

  useEffect(() => {
    if (!isDragging) return

    const handleMouseMove = (e: MouseEvent) => {
      handleScrub(e.clientX)
    }

    const handleMouseUp = () => {
      setIsDragging(false)
    }

    document.addEventListener("mousemove", handleMouseMove)
    document.addEventListener("mouseup", handleMouseUp)

    return () => {
      document.removeEventListener("mousemove", handleMouseMove)
      document.removeEventListener("mouseup", handleMouseUp)
    }
  }, [isDragging, handleScrub])

  return (
    <div
      aria-label={
        isRecordingComplete && !recording
          ? "Drag to scrub through recording"
          : undefined
      }
      aria-valuenow={
        isRecordingComplete && !recording ? viewPosition * 100 : undefined
      }
      aria-valuemin={isRecordingComplete && !recording ? 0 : undefined}
      aria-valuemax={isRecordingComplete && !recording ? 100 : undefined}
      className={cn(
        "relative flex items-center",
        isRecordingComplete && !recording && "cursor-pointer",
        className
      )}
      onMouseDown={handleMouseDown}
      ref={containerRef}
      role={isRecordingComplete && !recording ? "slider" : undefined}
      style={{ height: heightStyle }}
      tabIndex={isRecordingComplete && !recording ? 0 : undefined}
      {...props}
    >
      <canvas className="block h-full w-full" ref={canvasRef} />
    </div>
  )
}
```

---
