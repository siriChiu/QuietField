# API and module contracts

## Core audio types

```cpp
struct AudioBlockView {
  float* const* channels;
  int32_t channel_count;
  int32_t frame_count;
  int32_t sample_rate;
  int64_t frame_position;
  int64_t monotonic_time_ns;
};
```

No ownership or allocation is implied.

## Analysis frame

```cpp
struct SpectralFrame {
  uint64_t sequence;
  int64_t center_time_ns;
  std::span<const std::complex<float>> bins;
};
```

## RSVD configuration

```cpp
struct RsvdConfig {
  int target_rank;
  int oversampling;
  int power_iterations;
  uint64_t deterministic_seed;
};
```

## Component descriptor

```cpp
struct NoiseComponent {
  uint32_t id;
  float singular_value;
  float center_frequency_hz;
  float bandwidth_hz;
  float persistence;
  float low_frequency_ratio;
  float subspace_stability;
  float transient_penalty;
  float qualification_score;
};
```

## Immutable model snapshot

```cpp
struct NoiseModelSnapshot {
  uint64_t version;
  int64_t created_time_ns;
  int32_t sample_rate;
  int32_t fft_size;
  int32_t hop_size;
  float confidence;
  float valid_low_hz;
  float valid_high_hz;
  std::shared_ptr<const ModelPayload> payload;
};
```

The callback may only read the snapshot. Publication must not free memory still visible to the callback; use double buffering, epoch reclamation, or another verified real-time-safe scheme.

## DSP interfaces

```cpp
class StftProcessor {
 public:
  virtual void push(std::span<const float> mono) = 0;
  virtual bool pop(SpectralFrame& out) = 0;
};

class NoiseModelEstimator {
 public:
  virtual std::shared_ptr<const NoiseModelSnapshot>
  update(std::span<const SpectralFrame> window) = 0;
};

class FastNoisePredictor {
 public:
  virtual void setModel(const NoiseModelSnapshot* model) noexcept = 0;
  virtual void process(const AudioBlockView& input,
                       AudioBlockView& generated) noexcept = 0;
};
```

## Offline CLI contract

Example:

```bash
quietcore-lab analyze \
  --input fan.wav \
  --algorithm rsvd \
  --band 20:400 \
  --rank auto \
  --output results/fan-rsvd
```

Outputs:
- `original.wav`
- `estimated_noise.wav`
- `residual.wav`
- `metrics.json`
- `components.csv`
- `spectrogram.csv` or binary equivalent
- `run_config.json`
