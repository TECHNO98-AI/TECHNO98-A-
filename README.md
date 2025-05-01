# TECHNO98-A-def maximumPeople(p, x, y, r):
    from collections import defaultdict

    # Step 1: Pair towns and sort
    towns = sorted(zip(x, p))  # (position, population)
    town_positions = [pos for pos, _ in towns]
    town_populations = [pop for _, pop in towns]
    n = len(towns)

    # Step 2: For each town, keep track of how many clouds cover it
    cloud_cover_count = [0] * n
    cloud_to_towns = defaultdict(list)

    for cloud_index in range(len(y)):
        left = y[cloud_index] - r[cloud_index]
        right = y[cloud_index] + r[cloud_index]

        # Use binary search to find affected towns
        from bisect import bisect_left, bisect_right
        l = bisect_left(town_positions, left)
        r_ = bisect_right(town_positions, right) - 1

        for i in range(l, r_ + 1):
            cloud_cover_count[i] += 1
            cloud_to_towns[cloud_index].append(i)

    # Step 3: Calculate initial sunny population
    initial_sunny = sum(town_populations[i] for i in range(n) if cloud_cover_count[i] == 0)

    # Step 4: For each cloud, calculate how many people would be sunny if we remove this cloud
    max_gain = 0
    for cloud_index, town_indices in cloud_to_towns.items():
        gain = 0
        for i in town_indices:
            if cloud_cover_count[i] == 1:
                gain += town_populations[i]
        max_gain = max(max_gain, gain)

    return initial_sunny + max_gain

if __name__ == '__main__':
    import os
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    n = int(input().strip())

    p = list(map(int, input().rstrip().split()))
    x = list(map(int, input().rstrip().split()))

    m = int(input().strip())

    y = list(map(int, input().rstrip().split()))
    r = list(map(int, input().rstrip().split()))

    result = maximumPeople(p, x, y, r)

    fptr.write(str(result) + '\n')

    fptr.close()
