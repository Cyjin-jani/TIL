## 심플 오디오 플레이어 만들기
<br>

오디오를 사용하여 심플한 플레이어를 만들어보았다.
기능은 재생, 일시정지, 탐색, 현재 재생시간 표시, 전체 재생시간 표시의 기능을 갖추었다.

구현 방식은 다음과 같다.

```tsx
import { observer } from 'mobx-react-lite';
import { useState, useRef, useEffect, FC } from 'react'
import styled from 'styled-components';

import PlayingSrc from '../../assets/images/ic-small-fill-play-gray.png';
import StopSrc from '../../assets/images/ic-small-line-stop-gray.png';
import { useStores } from '../../stores/Context';

import Button from '../IconButton';
import Label from '../Label';

type Props = {
 url?: string;
}

const AudioPlayer:FC<Props> = ({ url }) => {
  const { musicStore } = useStores();
  const [duration, setDuration] = useState(0);
  const [currentTime, setCurrentTime] = useState(0);
  const audioPlayer = useRef<HTMLAudioElement | any>(null);
  const progressBar = useRef<HTMLInputElement>(null);

  useEffect(() => {
    let seconds;
    if(audioPlayer && audioPlayer.current) {
      audioPlayer.current.addEventListener('loadedmetadata', function() {
        seconds = Math.floor(audioPlayer.current.duration);
        setDuration(seconds);
      });
    }
    if (progressBar && progressBar.current) {
      progressBar.current.max = seconds+'';
    }
    if (musicStore.isPlaying) {
      audioPlayer.current.play();
    } else {
      audioPlayer.current.pause();
    }
    
  }, [musicStore.isPlaying, audioPlayer?.current?.loadedmetadata, audioPlayer?.current?.readyState]);


  useEffect(() => {
    resetTime();
  }, [url]);

  const calculateTime = (secs: number) => {
    const minutes = Math.floor(secs / 60);
    const returnedMinutes = minutes < 10 ? `0${minutes}` : `${minutes}`;
    const seconds = Math.floor(secs % 60);
    const returnedSeconds = seconds < 10 ? `0${seconds}` : `${seconds}`;
    return `${returnedMinutes}:${returnedSeconds}`;
  }

  const togglePlayPause = () => {
    musicStore.playToggle();
    
    if (musicStore.isPlaying) {
      audioPlayer.current.play();
    } else {
      audioPlayer.current.pause();
    }
  }

  const changeRange = (e: React.ChangeEvent<HTMLInputElement>) => {
    audioPlayer.current.currentTime = progressBar?.current?.value;
    changePlayerCurrentTime();
  }

  const changePlayerCurrentTime = () => {
    setCurrentTime(parseInt(progressBar?.current?.value || '0'));
  }

  const resetTime = () => {
    setCurrentTime(0);
    if (progressBar.current) {
      progressBar.current.value = '0';
    }
  }

  return (
    <Wrapper>
      <audio ref={audioPlayer} src={url} preload="metadata" onTimeUpdate={(e) => {
        setCurrentTime(e.currentTarget.currentTime);
      }}></audio>

      <ButtonWrapper>
        <Button iconSrc={musicStore.isPlaying ? StopSrc : PlayingSrc} handleClick={togglePlayPause} />
      </ButtonWrapper>
      
      {/* music title */}
      <TitleWrapper>
        <Label variant="subtitle">{musicStore.selectedOne?.title}</Label>
      </TitleWrapper>

      {/* current time */}
      <CurrentTimeWrapper>{calculateTime(currentTime)}</CurrentTimeWrapper>

      {/* progress bar */}
      <ProgressBar>
        <Bar type="range" ref={progressBar} value={currentTime} onChange={(e) => changeRange(e)} />
      </ProgressBar>

      {/* duration */}
      <DurationTimeWrapper>
        {(duration && !isNaN(duration)) && calculateTime(duration)}
      </DurationTimeWrapper>
    </Wrapper>
  )
}

const Wrapper = styled.div`
  display: flex;
  width: 100%;
  max-width: 1024px;
  margin: 0 auto;
  align-items: center;
  padding: 8px 24px;
`;

const ButtonWrapper = styled.div`
  margin-left: 24px;
`;

const TitleWrapper = styled.div`
  margin-right: 24px;
`;

const ProgressBar = styled.div``;

const Bar = styled.input`
  width: 300px;
`;

const CurrentTimeWrapper = styled.div`
  width: 50px;
  text-align: center;
  margin-right: 8px;
`;

const DurationTimeWrapper = styled.div`
  width: 50px;
  text-align: center;
  margin-left: 8px;  
`;


export default observer(AudioPlayer);
```

참고
https://developer.mozilla.org/ko/docs/Web/API/Web_Audio_API/Using_Web_Audio_API#more_examples